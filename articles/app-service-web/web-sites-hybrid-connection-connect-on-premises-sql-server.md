---
title: "aaaConnect tooon místnímu SQL serveru z webové aplikace v Azure App Service pomocí hybridních připojení"
description: "Vytvoření webové aplikace v Microsoft Azure a připojte ho tooan, místní databázi systému SQL Server"
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
ms.openlocfilehash: 2e8f8f7e0b9733cfb0433697615faba4358c6023
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-tooon-premises-sql-server-from-a-web-app-in-azure-app-service-using-hybrid-connections"></a><span data-ttu-id="51f29-103">Připojit tooon místnímu SQL serveru z webové aplikace v Azure App Service pomocí hybridních připojení</span><span class="sxs-lookup"><span data-stu-id="51f29-103">Connect tooon-premises SQL Server from a web app in Azure App Service using Hybrid Connections</span></span>
<span data-ttu-id="51f29-104">Hybridní připojení se můžete připojit [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) prostředky tooon místní webové aplikace, které používají statický port TCP.</span><span class="sxs-lookup"><span data-stu-id="51f29-104">Hybrid Connections can connect [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) Web Apps tooon-premises resources that use a static TCP port.</span></span> <span data-ttu-id="51f29-105">Podporované prostředky zahrnují Microsoft SQL Server, MySQL, webové rozhraní API HTTP, služby App Service a většina vlastních webových služeb.</span><span class="sxs-lookup"><span data-stu-id="51f29-105">Supported resources include Microsoft SQL Server, MySQL, HTTP Web APIs, App Service, and most custom Web Services.</span></span>

<span data-ttu-id="51f29-106">V tomto kurzu se dozvíte, jak toocreate aplikační službu webové aplikace v hello [portálu Azure](http://go.microsoft.com/fwlink/?LinkId=529715)se připojte hello webové aplikace tooyour místní databázi systému SQL Server pomocí nové funkce hybridní připojení hello, vytvořit jednoduché ASP.NET aplikace, který bude používat hello hybridní připojení a nasadit aplikace toohello hello webové aplikace App Service.</span><span class="sxs-lookup"><span data-stu-id="51f29-106">In this tutorial, you will learn how toocreate an App Service web app in hello [Azure Portal](http://go.microsoft.com/fwlink/?LinkId=529715), connect hello web app tooyour local on-premises SQL Server database using hello new Hybrid Connection feature, create a simple ASP.NET application that will use hello hybrid connection, and deploy hello application toohello App Service web app.</span></span> <span data-ttu-id="51f29-107">Hello dokončit webové aplikace v Azure uloží v databázi členství, která je místní přihlašovací údaje uživatele.</span><span class="sxs-lookup"><span data-stu-id="51f29-107">hello completed web app on Azure stores user credentials in a membership database that is on-premises.</span></span> <span data-ttu-id="51f29-108">Hello kurz předpokládá žádné předchozí zkušenosti s používáním Azure nebo v technologii ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="51f29-108">hello tutorial assumes no prior experience using Azure or ASP.NET.</span></span>

> [!NOTE]
> <span data-ttu-id="51f29-109">Pokud chcete, aby tooget začít s Azure App Service před registrací účtu Azure, přejděte příliš[vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/), kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service.</span><span class="sxs-lookup"><span data-stu-id="51f29-109">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="51f29-110">Nevyžaduje se žádná platební karta a nevzniká žádný závazek.</span><span class="sxs-lookup"><span data-stu-id="51f29-110">No credit cards required; no commitments.</span></span>
> 
> <span data-ttu-id="51f29-111">část webové aplikace Hello hello hybridní připojení funkce je k dispozici pouze v hello [portálu Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="51f29-111">hello Web Apps portion of hello Hybrid Connections feature is available only in hello [Azure Portal](https://portal.azure.com).</span></span> <span data-ttu-id="51f29-112">toocreate připojení ve službě BizTalk Services najdete v části [hybridní připojení](http://go.microsoft.com/fwlink/p/?LinkID=397274).</span><span class="sxs-lookup"><span data-stu-id="51f29-112">toocreate a connection in BizTalk Services, see [Hybrid Connections](http://go.microsoft.com/fwlink/p/?LinkID=397274).</span></span>  
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="51f29-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="51f29-113">Prerequisites</span></span>
<span data-ttu-id="51f29-114">toocomplete tohoto kurzu budete potřebovat hello následující produkty.</span><span class="sxs-lookup"><span data-stu-id="51f29-114">toocomplete this tutorial, you'll need hello following products.</span></span> <span data-ttu-id="51f29-115">Všechny jsou k dispozici v bezplatné verze, abyste mohli začít, vývoj pro Azure zcela zdarma.</span><span class="sxs-lookup"><span data-stu-id="51f29-115">All are available in free versions, so you can start developing for Azure entirely for free.</span></span>

* <span data-ttu-id="51f29-116">**Předplatné Azure** – bezplatné předplatné, najdete v části [bezplatná zkušební verze Azure](/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="51f29-116">**Azure subscription** - For a free subscription, see [Azure Free Trial](/pricing/free-trial/).</span></span>
* <span data-ttu-id="51f29-117">**Visual Studio 2013** -toodownload bezplatná zkušební verze produktu Visual Studio 2013, najdete v části [Visual Studio stáhne](http://www.visualstudio.com/downloads/download-visual-studio-vs).</span><span class="sxs-lookup"><span data-stu-id="51f29-117">**Visual Studio 2013** - toodownload a free trial version of Visual Studio 2013, see [Visual Studio Downloads](http://www.visualstudio.com/downloads/download-visual-studio-vs).</span></span> <span data-ttu-id="51f29-118">Než budete pokračovat, nainstalujte tento.</span><span class="sxs-lookup"><span data-stu-id="51f29-118">Install this before continuing.</span></span>
* <span data-ttu-id="51f29-119">**Rozhraní Microsoft .NET Framework 3.5 Service Pack 1** – Pokud je váš operační systém Windows 8.1, Windows Server 2012 R2, Windows 8, Windows Server 2012, Windows 7 nebo Windows Server 2008 R2, můžete ho povolit v Ovládacích panelech > programy a funkce > zapnout Funkce systému Windows nebo vypnout.</span><span class="sxs-lookup"><span data-stu-id="51f29-119">**Microsoft .NET Framework 3.5 Service Pack 1** - If your operating system is Windows 8.1, Windows Server 2012 R2, Windows 8, Windows Server 2012, Windows 7, or Windows Server 2008 R2, you can enable this in Control Panel > Programs and Features > Turn Windows features on or off.</span></span> <span data-ttu-id="51f29-120">Jinak, můžete ji stáhnout z hello [Microsoft Download Center](http://www.microsoft.com/download/en/details.aspx?displaylang=en&id=22).</span><span class="sxs-lookup"><span data-stu-id="51f29-120">Otherwise, you can download it from hello [Microsoft Download Center](http://www.microsoft.com/download/en/details.aspx?displaylang=en&id=22).</span></span>
* <span data-ttu-id="51f29-121">**SQL Server 2014 Express with Tools** -Microsoft SQL Server Express si zdarma stáhněte v hello [databáze platforma Microsoft webové stránky](http://www.microsoft.com/web/platform/database.aspx).</span><span class="sxs-lookup"><span data-stu-id="51f29-121">**SQL Server 2014 Express with Tools** - download Microsoft SQL Server Express for free at hello [Microsoft Web Platform Database page](http://www.microsoft.com/web/platform/database.aspx).</span></span> <span data-ttu-id="51f29-122">Zvolte hello **Express** (ne LocalDB) verze.</span><span class="sxs-lookup"><span data-stu-id="51f29-122">Choose hello **Express** (not LocalDB) version.</span></span> <span data-ttu-id="51f29-123">Hello **Express with Tools** verze zahrnuje SQL Server Management Studio, které budete používat v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="51f29-123">hello **Express with Tools** version includes SQL Server Management Studio, which you will use in this tutorial.</span></span>
* <span data-ttu-id="51f29-124">**SQL Server Management Studio Express** – je součástí systému SQL Server 2014 Express hello nástroje pro stažení výše uvedených, ale pokud budete potřebovat tooinstall ho samostatně, můžete stáhnout a nainstalovat z hello [SQL Server Express stránce pro stažení](http://www.microsoft.com/web/platform/database.aspx).</span><span class="sxs-lookup"><span data-stu-id="51f29-124">**SQL Server Management Studio Express** - This is included with hello SQL Server 2014 Express with Tools download mentioned above, but if you need tooinstall it separately, you can download and install it from hello [SQL Server Express download page](http://www.microsoft.com/web/platform/database.aspx).</span></span>

<span data-ttu-id="51f29-125">Hello kurz předpokládá, že máte předplatné Azure, jestli máte nainstalovanou sadu Visual Studio 2013, a že jste nainstalovali nebo povoleno rozhraní .NET Framework 3.5.</span><span class="sxs-lookup"><span data-stu-id="51f29-125">hello tutorial assumes that you have an Azure subscription, that you have installed Visual Studio 2013, and that you have installed or enabled .NET Framework 3.5.</span></span> <span data-ttu-id="51f29-126">Hello kurzu se dozvíte, jak funkce tooinstall SQL Server 2014 Express v konfiguraci, který pracuje s hello Azure hybridní připojení (výchozí instanci s statický port TCP).</span><span class="sxs-lookup"><span data-stu-id="51f29-126">hello tutorial shows you how tooinstall SQL Server 2014 Express in a configuration that works well with hello Azure Hybrid Connections feature (a default instance with a static TCP port).</span></span> <span data-ttu-id="51f29-127">Před zahájením hello kurzu, stáhněte z umístění hello uvedených výše, pokud nemáte nainstalovaný server SQL Server SQL Server 2014 Express with Tools.</span><span class="sxs-lookup"><span data-stu-id="51f29-127">Before starting hello tutorial, download SQL Server 2014 Express with Tools from hello location mentioned above if you do not have SQL Server installed.</span></span>

### <a name="notes"></a><span data-ttu-id="51f29-128">Poznámky</span><span class="sxs-lookup"><span data-stu-id="51f29-128">Notes</span></span>
<span data-ttu-id="51f29-129">toouse místní SQL Server nebo SQL Server Express databáze s hybridní připojení TCP/IP musí toobe povoleno na statický port.</span><span class="sxs-lookup"><span data-stu-id="51f29-129">toouse an on-premises SQL Server or SQL Server Express database with a hybrid connection, TCP/IP needs toobe enabled on a static port.</span></span> <span data-ttu-id="51f29-130">Výchozí instance na serveru SQL Server používají statický port 1433, zatímco pojmenovaných instancí nepodporují.</span><span class="sxs-lookup"><span data-stu-id="51f29-130">Default instances on SQL Server use static port 1433, whereas named instances do not.</span></span>

<span data-ttu-id="51f29-131">Hello počítač, na který instalujete hello místní správce hybridního připojení agenta:</span><span class="sxs-lookup"><span data-stu-id="51f29-131">hello computer on which you install hello on-premises Hybrid Connection Manager agent:</span></span>

* <span data-ttu-id="51f29-132">Musí mít tooAzure odchozí připojení přes:</span><span class="sxs-lookup"><span data-stu-id="51f29-132">Must have outbound connectivity tooAzure over:</span></span>

| <span data-ttu-id="51f29-133">Port</span><span class="sxs-lookup"><span data-stu-id="51f29-133">Port</span></span> | <span data-ttu-id="51f29-134">Proč</span><span class="sxs-lookup"><span data-stu-id="51f29-134">Why</span></span> |
| --- | --- |
| <span data-ttu-id="51f29-135">80</span><span class="sxs-lookup"><span data-stu-id="51f29-135">80</span></span> |<span data-ttu-id="51f29-136">**Požadované** pro port HTTP pro ověření certifikátu a volitelně pro datové připojení.</span><span class="sxs-lookup"><span data-stu-id="51f29-136">**Required** for HTTP port for certificate validation and optionally for data connectivity.</span></span> |
| <span data-ttu-id="51f29-137">443</span><span class="sxs-lookup"><span data-stu-id="51f29-137">443</span></span> |<span data-ttu-id="51f29-138">**Volitelné** pro datové připojení.</span><span class="sxs-lookup"><span data-stu-id="51f29-138">**Optional** for data connectivity.</span></span> <span data-ttu-id="51f29-139">Pokud too443 odchozí připojení není k dispozici, použije se TCP port 80.</span><span class="sxs-lookup"><span data-stu-id="51f29-139">If outbound connectivity too443 is unavailable, TCP port 80 is used.</span></span> |
| <span data-ttu-id="51f29-140">5671 a 9352</span><span class="sxs-lookup"><span data-stu-id="51f29-140">5671 and 9352</span></span> |<span data-ttu-id="51f29-141">**Doporučená** ale volitelné pro datové připojení.</span><span class="sxs-lookup"><span data-stu-id="51f29-141">**Recommended** but Optional for data connectivity.</span></span> <span data-ttu-id="51f29-142">Všimněte si, že tento režim obvykle dává vyšší propustnost.</span><span class="sxs-lookup"><span data-stu-id="51f29-142">Note this mode usually yields higher throughput.</span></span> <span data-ttu-id="51f29-143">Pokud porty toothese odchozí připojení není k dispozici, použije se port 443 protokolu TCP.</span><span class="sxs-lookup"><span data-stu-id="51f29-143">If outbound connectivity toothese ports is unavailable, TCP port 443 is used.</span></span> |

* <span data-ttu-id="51f29-144">Musí být schopný tooreach hello *hostname*:*ČísloPortu* vaše místní prostředku.</span><span class="sxs-lookup"><span data-stu-id="51f29-144">Must be able tooreach hello *hostname*:*portnumber* of your on-premises resource.</span></span>

<span data-ttu-id="51f29-145">Hello postup v tomto článku předpokládá, že používáte prohlížeč hello z hello počítače, který bude hostitelem hello místní hybridní připojení agenta.</span><span class="sxs-lookup"><span data-stu-id="51f29-145">hello steps in this article assume that you are using hello browser from hello computer that will host hello on-premises hybrid connection agent.</span></span>

<span data-ttu-id="51f29-146">Pokud už máte nainstalovaný v konfiguraci a v prostředí, které splňuje podmínky hello popsané výše server SQL Server, můžete přeskočit a začínat [vytvořit databázi systému SQL Server na místě](#CreateSQLDB).</span><span class="sxs-lookup"><span data-stu-id="51f29-146">If you already have SQL Server installed in a configuration and in an environment that meets hello conditions described above, you can skip ahead and start with [Create a SQL Server database on-premises](#CreateSQLDB).</span></span>

<a name="InstallSQL"></a>

## <a name="a-install-sql-server-express-enable-tcpip-and-create-a-sql-server-database-on-premises"></a><span data-ttu-id="51f29-147">A.</span><span class="sxs-lookup"><span data-stu-id="51f29-147">A.</span></span> <span data-ttu-id="51f29-148">Nainstalujte systém SQL Server Express, povolte protokol TCP/IP a vytvořit databázi systému SQL Server na místě</span><span class="sxs-lookup"><span data-stu-id="51f29-148">Install SQL Server Express, enable TCP/IP, and create a SQL Server database on-premises</span></span>
<span data-ttu-id="51f29-149">V této části se dozvíte, jak tooinstall SQL Server Express, povolte protokol TCP/IP a vytvořit databázi tak, aby webová aplikace bude fungovat s hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="51f29-149">This section shows you how tooinstall SQL Server Express, enable TCP/IP, and create a database so that your web application will work with hello Azure Portal.</span></span>

### <a name="install-sql-server-express"></a><span data-ttu-id="51f29-150">Nainstalujte SQL Server Express.</span><span class="sxs-lookup"><span data-stu-id="51f29-150">Install SQL Server Express</span></span>
1. <span data-ttu-id="51f29-151">SQL Server Express, tooinstall spustit hello **SQLEXPRWT_x64_ENU.exe** nebo **SQLEXPR_x86_ENU.exe** soubor, který jste stáhli.</span><span class="sxs-lookup"><span data-stu-id="51f29-151">tooinstall SQL Server Express, run hello **SQLEXPRWT_x64_ENU.exe** or **SQLEXPR_x86_ENU.exe** file that you downloaded.</span></span> <span data-ttu-id="51f29-152">Zobrazí se Průvodce Hello Centrum instalace SQL serveru.</span><span class="sxs-lookup"><span data-stu-id="51f29-152">hello SQL Server Installation Center wizard appears.</span></span>
   
    ![Instalace serveru SQL][SQLServerInstall]
2. <span data-ttu-id="51f29-154">Zvolte **nová instalace SQL serveru samostatné, nebo přidejte existující instalace funkce tooan**.</span><span class="sxs-lookup"><span data-stu-id="51f29-154">Choose **New SQL Server stand-alone installation or add features tooan existing installation**.</span></span> <span data-ttu-id="51f29-155">Postupujte podle pokynů hello, přijetí hello výchozí možnosti a nastavení, dokud nezískáte toohello **konfigurace Instance** stránky.</span><span class="sxs-lookup"><span data-stu-id="51f29-155">Follow hello instructions, accepting hello default choices and settings, until you get toohello **Instance Configuration** page.</span></span>
3. <span data-ttu-id="51f29-156">Na hello **konfigurace Instance** vyberte **výchozí instance**.</span><span class="sxs-lookup"><span data-stu-id="51f29-156">On hello **Instance Configuration** page, choose **Default instance**.</span></span>
   
    ![Vyberte výchozí instanci][ChooseDefaultInstance]
   
    <span data-ttu-id="51f29-158">Ve výchozím nastavení hello výchozí instance systému SQL Server čeká na požadavky od klientů systému SQL Server na statický port 1433, což je co hello hybridní připojení funkce vyžaduje.</span><span class="sxs-lookup"><span data-stu-id="51f29-158">By default, hello default instance of SQL Server listens for requests from SQL Server clients on static port 1433, which is what hello Hybrid Connections feature requires.</span></span> <span data-ttu-id="51f29-159">Pojmenované instance používají dynamické porty a UDP, které nejsou podporovány hybridní připojení.</span><span class="sxs-lookup"><span data-stu-id="51f29-159">Named instances use dynamic ports and UDP, which are not supported by Hybrid Connections.</span></span>
4. <span data-ttu-id="51f29-160">Přijměte výchozí hodnoty hello na hello **konfigurace serveru** stránky.</span><span class="sxs-lookup"><span data-stu-id="51f29-160">Accept hello defaults on hello **Server Configuration** page.</span></span>
5. <span data-ttu-id="51f29-161">Na hello **konfigurace databázového stroje** v části **režim ověřování**, zvolte **smíšený režim (ověřování systému SQL Server a ověřování systému Windows)**a zadejte heslo.</span><span class="sxs-lookup"><span data-stu-id="51f29-161">On hello **Database Engine Configuration** page, under **Authentication Mode**, choose **Mixed Mode (SQL Server authentication and Windows authentication)**, and provide a password.</span></span>
   
    ![Zvolte ve smíšeném režimu][ChooseMixedMode]
   
    <span data-ttu-id="51f29-163">V tomto kurzu budete používat ověřování serveru SQL Server.</span><span class="sxs-lookup"><span data-stu-id="51f29-163">In this tutorial, you will be using SQL Server authentication.</span></span> <span data-ttu-id="51f29-164">Být jisti tooremember hello heslo, které zadáte, protože jej budete potřebovat později.</span><span class="sxs-lookup"><span data-stu-id="51f29-164">Be sure tooremember hello password that you provide, because you will need it later.</span></span>
6. <span data-ttu-id="51f29-165">Projděte hello zbytek hello Průvodce toocomplete hello instalace.</span><span class="sxs-lookup"><span data-stu-id="51f29-165">Step through hello rest of hello wizard toocomplete hello installation.</span></span>

### <a name="enable-tcpip"></a><span data-ttu-id="51f29-166">Povolte protokol TCP/IP</span><span class="sxs-lookup"><span data-stu-id="51f29-166">Enable TCP/IP</span></span>
<span data-ttu-id="51f29-167">tooenable TCP/IP, budete používat SQL Server Configuration Manager, který byl nainstalován při instalaci systému SQL Server Express.</span><span class="sxs-lookup"><span data-stu-id="51f29-167">tooenable TCP/IP, you will use SQL Server Configuration Manager, which was installed when you installed SQL Server Express.</span></span> <span data-ttu-id="51f29-168">Postupujte podle kroků hello v [povolit síťový protokol TCP/IP pro SQL Server](http://technet.microsoft.com/library/hh231672%28v=sql.110%29.aspx) než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="51f29-168">Follow hello steps in [Enable TCP/IP Network Protocol for SQL Server](http://technet.microsoft.com/library/hh231672%28v=sql.110%29.aspx) before continuing.</span></span>

<a name="CreateSQLDB"></a>

### <a name="create-a-sql-server-database-on-premises"></a><span data-ttu-id="51f29-169">Vytvořit databázi systému SQL Server na místě</span><span class="sxs-lookup"><span data-stu-id="51f29-169">Create a SQL Server database on-premises</span></span>
<span data-ttu-id="51f29-170">Webové aplikace Visual Studio vyžaduje členství v databázi, která je přístupná v Azure.</span><span class="sxs-lookup"><span data-stu-id="51f29-170">Your Visual Studio web application requires a membership database that can be accessed by Azure.</span></span> <span data-ttu-id="51f29-171">To vyžaduje databázi systému SQL Server nebo SQL Server Express (ne hello LocalDB databáze hello používá šablony MVC ve výchozím nastavení), takže vytvoříte databázi členství hello Další.</span><span class="sxs-lookup"><span data-stu-id="51f29-171">This requires a SQL Server or SQL Server Express database (not hello LocalDB database that hello MVC template uses by default), so you'll create hello membership database next.</span></span>

1. <span data-ttu-id="51f29-172">V systému SQL Server Management Studio připojte toohello systému SQL Server, kterou jste právě nainstalovali.</span><span class="sxs-lookup"><span data-stu-id="51f29-172">In SQL Server Management Studio, connect toohello SQL Server you just installed.</span></span> <span data-ttu-id="51f29-173">(Pokud hello **připojit tooServer** dialogové okno nezobrazí automaticky, přejděte příliš**Průzkumník objektů** v levém podokně hello, klikněte na **Connect**a potom klikněte na **Databáze modul**.) ![Připojit tooServer][SSMSConnectToServer]</span><span class="sxs-lookup"><span data-stu-id="51f29-173">(If hello **Connect tooServer** dialog does not appear automatically, navigate too**Object Explorer** in hello left pane, click **Connect**, and then click **Database Engine**.) ![Connect tooServer][SSMSConnectToServer]</span></span>
   
    <span data-ttu-id="51f29-174">Pro **typ serveru**, zvolte **databázový stroj**.</span><span class="sxs-lookup"><span data-stu-id="51f29-174">For **Server type**, choose **Database Engine**.</span></span> <span data-ttu-id="51f29-175">Pro **název serveru**, můžete použít **localhost** nebo hello název hello počítače, který používáte.</span><span class="sxs-lookup"><span data-stu-id="51f29-175">For **Server name**, you can use **localhost** or hello name of hello computer that you are using.</span></span> <span data-ttu-id="51f29-176">Zvolte **ověřování serveru SQL Server**a potom se přihlaste se pomocí hello sa uživatelské jméno a heslo hello, kterou jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="51f29-176">Choose **SQL Server authentication**, and then log in with hello sa user name and hello password that you created earlier.</span></span>
2. <span data-ttu-id="51f29-177">toocreate novou databázi pomocí SQL Server Management Studio, klikněte pravým tlačítkem na **databáze** v Průzkumníku objektů a potom klikněte na **novou databázi**.</span><span class="sxs-lookup"><span data-stu-id="51f29-177">toocreate a new database by using SQL Server Management Studio, right-click **Databases** in Object Explorer, and then click **New Database**.</span></span>
   
    ![Vytvoření nové databáze][SSMScreateNewDB]
3. <span data-ttu-id="51f29-179">V hello **novou databázi** dialogové okno, zadejte MembershipDB pro hello název databáze a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="51f29-179">In hello **New Database** dialog, enter MembershipDB for hello database name, and then click **OK**.</span></span>
   
    ![Zadejte název databáze][SSMSprovideDBname]
   
    <span data-ttu-id="51f29-181">Poznámka: všechny změny toohello databáze v tuto chvíli Nedělejte.</span><span class="sxs-lookup"><span data-stu-id="51f29-181">Note that you do not make any changes toohello database at this point.</span></span> <span data-ttu-id="51f29-182">informace o členství Hello budou automaticky později přidány webovou aplikací, když jej spustíte.</span><span class="sxs-lookup"><span data-stu-id="51f29-182">hello membership information will be added automatically later by your web application when you run it.</span></span>
4. <span data-ttu-id="51f29-183">V Průzkumníku objektů, pokud rozbalíte **databáze**, zobrazí se, že hello členství databáze byla vytvořena.</span><span class="sxs-lookup"><span data-stu-id="51f29-183">In Object Explorer, if you expand **Databases**, you will see that hello membership database has been created.</span></span>
   
    ![MembershipDB vytvořen][SSMSMembershipDBCreated]

<a name="CreateSite"></a>

## <a name="b-create-a-web-app-in-hello-azure-portal"></a><span data-ttu-id="51f29-185">B.</span><span class="sxs-lookup"><span data-stu-id="51f29-185">B.</span></span> <span data-ttu-id="51f29-186">Vytvoření webové aplikace v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="51f29-186">Create a web app in hello Azure Portal</span></span>
> [!NOTE]
> <span data-ttu-id="51f29-187">Pokud jste již vytvořili webovou aplikaci v hello portálu Azure, že chcete toouse pro účely tohoto kurzu, můžete přeskočit příliš[vytvořit hybridní připojení a služby BizTalk](#CreateHC) a pokračovat dál.</span><span class="sxs-lookup"><span data-stu-id="51f29-187">If you have already created a web app in hello Azure Portal that you want toouse for this tutorial, you can skip ahead too[Create a Hybrid Connection and a BizTalk Service](#CreateHC) and continue from there.</span></span>
> 
> 

1. <span data-ttu-id="51f29-188">V hello [portálu Azure](https://portal.azure.com), klikněte na tlačítko **nový** > **Web + mobilní** > **webové aplikace**.</span><span class="sxs-lookup"><span data-stu-id="51f29-188">In hello [Azure Portal](https://portal.azure.com), click **New** > **Web + Mobile** > **Web app**.</span></span>
   
    ![tlačítko Nový][New]
2. <span data-ttu-id="51f29-190">Konfigurace webové aplikace a pak klikněte na **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="51f29-190">Configure your web app, and then click **Create**.</span></span>
   
    ![Název webu][WebsiteCreationBlade]
3. <span data-ttu-id="51f29-192">Po chvíli se hello webové aplikace se vytvoří a otevře se okno jeho webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="51f29-192">After a few moments, hello web app is created and its web app blade appears.</span></span> <span data-ttu-id="51f29-193">okno Hello se svisle posouvatelným řídicí panel, který vám umožní spravovat webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="51f29-193">hello blade is a vertically scrollable dashboard that lets you manage your web app.</span></span>
   
    ![Spuštění webu][WebSiteRunningBlade]
   
    <span data-ttu-id="51f29-195">tooverify hello webová aplikace je za provozu, můžete kliknout na hello **Procházet** ikonu toodisplay hello výchozí stránky.</span><span class="sxs-lookup"><span data-stu-id="51f29-195">tooverify hello web app is live, you can click hello **Browse** icon toodisplay hello default page.</span></span>

<span data-ttu-id="51f29-196">V dalším kroku vytvoříte hybridní připojení a služby BizTalk pro hello webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="51f29-196">Next, you will create a hybrid connection and a BizTalk service for hello web app.</span></span>

<a name="CreateHC"></a>

## <a name="c-create-a-hybrid-connection-and-a-biztalk-service"></a><span data-ttu-id="51f29-197">C.</span><span class="sxs-lookup"><span data-stu-id="51f29-197">C.</span></span> <span data-ttu-id="51f29-198">Vytvoření hybridní připojení a služby BizTalk</span><span class="sxs-lookup"><span data-stu-id="51f29-198">Create a Hybrid Connection and a BizTalk Service</span></span>
1. <span data-ttu-id="51f29-199">Zpět v hello portálu, přejděte toosettings a klikněte na tlačítko **sítě** > **nakonfigurovat koncové body hybridního připojení**.</span><span class="sxs-lookup"><span data-stu-id="51f29-199">Back in hello Portal, go toosettings and click **Networking** > **Configure your hybrid connection endpoints**.</span></span>
   
    ![Hybridní připojení][CreateHCHCIcon]
2. <span data-ttu-id="51f29-201">V okně připojení hybridní hello, klikněte na tlačítko **přidat** > **nové hybridní připojení**.</span><span class="sxs-lookup"><span data-stu-id="51f29-201">On hello Hybrid connections blade, click **Add** > **New hybrid connection**.</span></span>
3. <span data-ttu-id="51f29-202">Na hello **vytvořit hybridní připojení** okno:</span><span class="sxs-lookup"><span data-stu-id="51f29-202">On hello **Create hybrid connection** blade:</span></span>
   
   * <span data-ttu-id="51f29-203">Pro **název**, zadejte název připojení hello.</span><span class="sxs-lookup"><span data-stu-id="51f29-203">For **Name**, provide a name for hello connection.</span></span>
   * <span data-ttu-id="51f29-204">Pro **Hostname**, zadejte název počítače hello hostitelského počítače systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="51f29-204">For **Hostname**, enter hello computer name of your SQL Server host computer.</span></span>
   * <span data-ttu-id="51f29-205">Pro **Port**, zadejte 1433 (hello výchozí port pro SQL Server).</span><span class="sxs-lookup"><span data-stu-id="51f29-205">For **Port**, enter 1433 (hello default port for SQL Server).</span></span>
   * <span data-ttu-id="51f29-206">Klikněte na tlačítko **služby BizTalk** > **novou službu BizTalk** a zadejte název pro hello služby BizTalk.</span><span class="sxs-lookup"><span data-stu-id="51f29-206">Click **BizTalk Service** > **New BizTalk Service** and enter a name for hello BizTalk service.</span></span>
     
     ![Vytvořit hybridní připojení.][TwinCreateHCBlades]
4. <span data-ttu-id="51f29-208">Klikněte na tlačítko **OK** dvakrát.</span><span class="sxs-lookup"><span data-stu-id="51f29-208">Click **OK** twice.</span></span>
   
    <span data-ttu-id="51f29-209">Když hello dokončení procesu hello **oznámení** oblasti blikat zelená **úspěch** a hello **hybridní připojení** okno se zobrazí hello nové hybridní připojení s Hello stav jako **Nepřipojeno**.</span><span class="sxs-lookup"><span data-stu-id="51f29-209">When hello process completes, hello **Notifications** area will flash a green **SUCCESS** and hello **Hybrid connection** blade will show hello new hybrid connection with hello status as **Not connected**.</span></span>
   
    ![Vytvoření jednoho hybridní připojení][CreateHCOneConnectionCreated]

<span data-ttu-id="51f29-211">V tomto okamžiku jste dokončili důležitou součástí infrastruktury hello cloudu hybridní připojení.</span><span class="sxs-lookup"><span data-stu-id="51f29-211">At this point, you have completed an important part of hello cloud hybrid connection infrastructure.</span></span> <span data-ttu-id="51f29-212">V dalším kroku vytvoříte odpovídající část místně.</span><span class="sxs-lookup"><span data-stu-id="51f29-212">Next, you will create a corresponding on-premises piece.</span></span>

<a name="InstallHCM"></a>

## <a name="d-install-hello-on-premises-hybrid-connection-manager-toocomplete-hello-connection"></a><span data-ttu-id="51f29-213">D.</span><span class="sxs-lookup"><span data-stu-id="51f29-213">D.</span></span> <span data-ttu-id="51f29-214">Instalace hello místní správce hybridního připojení toocomplete hello připojení</span><span class="sxs-lookup"><span data-stu-id="51f29-214">Install hello on-premises Hybrid Connection Manager toocomplete hello connection</span></span>
[!INCLUDE [app-service-hybrid-connections-manager-install](../../includes/app-service-hybrid-connections-manager-install.md)]

<span data-ttu-id="51f29-215">Teď dokončení tuto infrastrukturu hello hybridní připojení se vytvoří webovou aplikaci, která jej používá.</span><span class="sxs-lookup"><span data-stu-id="51f29-215">Now that hello hybrid connection infrastructure is complete, you will create a web application that uses it.</span></span>

<a name="CreateASPNET"></a>

## <a name="e-create-a-basic-aspnet-web-project-edit-hello-database-connection-string-and-run-hello-project-locally"></a><span data-ttu-id="51f29-216">E.</span><span class="sxs-lookup"><span data-stu-id="51f29-216">E.</span></span> <span data-ttu-id="51f29-217">Vytvořit základní projekt ASP.NET, upravit připojovací řetězec databáze hello a spustit projekt hello místně</span><span class="sxs-lookup"><span data-stu-id="51f29-217">Create a basic ASP.NET web project, edit hello database connection string, and run hello project locally</span></span>
### <a name="create-a-basic-aspnet-project"></a><span data-ttu-id="51f29-218">Vytvoření základního projektu ASP.NET</span><span class="sxs-lookup"><span data-stu-id="51f29-218">Create a basic ASP.NET project</span></span>
1. <span data-ttu-id="51f29-219">V sadě Visual Studio na hello **souboru** nabídky, vytvořte nový projekt:</span><span class="sxs-lookup"><span data-stu-id="51f29-219">In Visual Studio, on hello **File** menu, create a new Project:</span></span>
   
    ![Nový projekt Visual Studio][HCVSNewProject]
2. <span data-ttu-id="51f29-221">V hello **šablony** části hello **nový projekt** dialogovém okně, vyberte **webové** a zvolte **webové aplikace ASP.NET**a pak klikněte na tlačítko  **OK**.</span><span class="sxs-lookup"><span data-stu-id="51f29-221">In hello **Templates** section of hello **New Project** dialog, select **Web** and choose **ASP.NET Web Application**, and then click **OK**.</span></span>
   
    ![Vyberte webové aplikace ASP.NET][HCVSChooseASPNET]
3. <span data-ttu-id="51f29-223">V hello **nový projekt ASP.NET** dialogovém okně, vyberte **MVC**a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="51f29-223">In hello **New ASP.NET Project** dialog, choose **MVC**, and then click **OK**.</span></span>
   
    ![Zvolte MVC][HCVSChooseMVC]
4. <span data-ttu-id="51f29-225">Po vytvoření projektu hello, zobrazí se stránka readme aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="51f29-225">When hello project has been created, hello application readme page appears.</span></span> <span data-ttu-id="51f29-226">Ještě nespouštějte hello webového projektu.</span><span class="sxs-lookup"><span data-stu-id="51f29-226">Do not run hello web project yet.</span></span>
   
    ![Stránce Readme][HCVSReadmePage]

### <a name="edit-hello-database-connection-string-for-hello-application"></a><span data-ttu-id="51f29-228">Upravit připojovací řetězec databáze hello aplikace hello</span><span class="sxs-lookup"><span data-stu-id="51f29-228">Edit hello database connection string for hello application</span></span>
<span data-ttu-id="51f29-229">V tomto kroku upravíte hello připojovací řetězec, který sděluje aplikace kde toofind místní systém SQL Server Express databáze.</span><span class="sxs-lookup"><span data-stu-id="51f29-229">In this step, you edit hello connection string that tells your application where toofind your local SQL Server Express database.</span></span> <span data-ttu-id="51f29-230">Hello připojovací řetězec je v souboru Web.config aplikace hello, který obsahuje informace o konfiguraci pro aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="51f29-230">hello connection string is in hello application's Web.config file, which contains configuration information for hello application.</span></span>

> [!NOTE]
> <span data-ttu-id="51f29-231">tooensure, které vaše aplikace používá hello databáze, kterou jste vytvořili v systému SQL Server Express a ne hello, jeden v sadě Visual Studio výchozí LocalDB, je důležité, provedení tohoto kroku před spuštěním projektu.</span><span class="sxs-lookup"><span data-stu-id="51f29-231">tooensure that your application uses hello database that you created in SQL Server Express, and not hello one in Visual Studio's default LocalDB, it is important that you complete this step before running your project.</span></span>
> 
> 

1. <span data-ttu-id="51f29-232">V Průzkumníku řešení poklikejte na soubor Web.config hello.</span><span class="sxs-lookup"><span data-stu-id="51f29-232">In Solution Explorer, double-click hello Web.config file.</span></span>
   
    ![Soubor web.config][HCVSChooseWebConfig]
2. <span data-ttu-id="51f29-234">Upravit hello **connectionStrings** části toopoint toohello databáze systému SQL Server na místním počítači, řiďte se syntaxí hello v hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="51f29-234">Edit hello **connectionStrings** section toopoint toohello SQL Server database on your local machine, following hello syntax in hello following example:</span></span>
   
    ![Připojovací řetězec][HCVSConnectionString]
   
    <span data-ttu-id="51f29-236">Při sestavování hello připojovací řetězec, mějte na paměti hello následující:</span><span class="sxs-lookup"><span data-stu-id="51f29-236">When composing hello connection string, keep in mind hello following:</span></span>
   
   * <span data-ttu-id="51f29-237">Pokud se připojujete tooa pojmenovanou instanci namísto výchozí instance (například YourServer\SQLEXPRESS), je nutné nakonfigurovat porty statické toouse vašeho systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="51f29-237">If you are connecting tooa named instance instead of a default instance (for example, YourServer\SQLEXPRESS), you must configure your SQL Server toouse static ports.</span></span> <span data-ttu-id="51f29-238">Informace o konfiguraci statických portů najdete v tématu [jak tooconfigure toolisten systému SQL Server na určitém portu](http://support.microsoft.com/kb/823938).</span><span class="sxs-lookup"><span data-stu-id="51f29-238">For information on configuring static ports, see [How tooconfigure SQL Server toolisten on a specific port](http://support.microsoft.com/kb/823938).</span></span> <span data-ttu-id="51f29-239">Ve výchozím nastavení používají pojmenované instance UDP a dynamické porty, které nejsou podporovány hybridní připojení.</span><span class="sxs-lookup"><span data-stu-id="51f29-239">By default, named instances use UDP and dynamic ports, which are not supported by Hybrid Connections.</span></span>
   * <span data-ttu-id="51f29-240">Doporučuje se, že zadáváte hello portu (ve výchozím nastavení, jak je znázorněno v příkladu hello 1433) na hello připojovací řetězec, abyste se ujistili, že místní systém SQL Server má TCP povoleno a používá správný port hello.</span><span class="sxs-lookup"><span data-stu-id="51f29-240">It is recommended that you specify hello port (1433 by default, as shown in hello example) on hello connection string so that you can be sure that your local SQL Server has TCP enabled and is using hello correct port.</span></span>
   * <span data-ttu-id="51f29-241">Mějte na paměti, tooconnect toouse ověřování systému SQL Server, zadání hello ID uživatele a heslo v připojovacím řetězci.</span><span class="sxs-lookup"><span data-stu-id="51f29-241">Remember toouse SQL Server Authentication tooconnect, specifying hello user ID and password in your connection string.</span></span>
3. <span data-ttu-id="51f29-242">Klikněte na tlačítko **Uložit** v souboru Web.config hello toosave Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="51f29-242">Click **Save** in Visual Studio toosave hello Web.config file.</span></span>

### <a name="run-hello-project-locally-and-register-a-new-user"></a><span data-ttu-id="51f29-243">Spusťte projekt hello místně a registraci nového uživatele</span><span class="sxs-lookup"><span data-stu-id="51f29-243">Run hello project locally and register a new user</span></span>
1. <span data-ttu-id="51f29-244">Teď spusťte místně nový webový projekt kliknutím na tlačítko Procházet hello pod ladění.</span><span class="sxs-lookup"><span data-stu-id="51f29-244">Now, run your new web project locally by clicking hello browse button under Debug.</span></span> <span data-ttu-id="51f29-245">Tento příklad používá aplikace Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="51f29-245">This example uses Internet Explorer.</span></span>
   
    ![Spusťte projekt][HCVSRunProject]
2. <span data-ttu-id="51f29-247">V horní pravé hello hello výchozí webové stránky, zvolte **zaregistrovat** tooregister nový účet:</span><span class="sxs-lookup"><span data-stu-id="51f29-247">On hello upper right of hello default web page, choose **Register** tooregister a new account:</span></span>
   
    ![Zaregistrovat nový účet][HCVSRegisterLocally]
3. <span data-ttu-id="51f29-249">Zadejte uživatelské jméno a heslo:</span><span class="sxs-lookup"><span data-stu-id="51f29-249">Enter a user name and password:</span></span>
   
    ![Zadejte uživatelské jméno a heslo][HCVSCreateNewAccount]
   
    <span data-ttu-id="51f29-251">Toto automaticky vytvoří databázi na vaše místní systém SQL Server, který obsahuje informace o členství hello pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="51f29-251">This automatically creates a database on your local SQL Server that holds hello membership information for your application.</span></span> <span data-ttu-id="51f29-252">Jedna z tabulek hello (**dbo. AspNetUsers**) blokování webové aplikace jako hello ta, která jste zadali přihlašovací údaje uživatele.</span><span class="sxs-lookup"><span data-stu-id="51f29-252">One of hello tables (**dbo.AspNetUsers**) holds web app user credentials like hello ones that you just entered.</span></span> <span data-ttu-id="51f29-253">Zobrazí se tato tabulka později v kurzu hello.</span><span class="sxs-lookup"><span data-stu-id="51f29-253">You will see this table later in hello tutorial.</span></span>
4. <span data-ttu-id="51f29-254">Zavřete okno prohlížeče hello hello výchozí webové stránky.</span><span class="sxs-lookup"><span data-stu-id="51f29-254">Close hello browser window of hello default web page.</span></span> <span data-ttu-id="51f29-255">To zastaví hello aplikace v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="51f29-255">This stops hello application in Visual Studio.</span></span>

<span data-ttu-id="51f29-256">Jsou nyní připraveny pro další krok text hello, což je tooAzure aplikace hello toopublish a otestovat.</span><span class="sxs-lookup"><span data-stu-id="51f29-256">You are now ready for hello next step, which is toopublish hello application tooAzure and test it.</span></span>

<a name="PubNTest"></a>

## <a name="f-publish-hello-web-application-tooazure-and-test-it"></a><span data-ttu-id="51f29-257">F.</span><span class="sxs-lookup"><span data-stu-id="51f29-257">F.</span></span> <span data-ttu-id="51f29-258">Publikování hello webové aplikace tooAzure a otestovat ji</span><span class="sxs-lookup"><span data-stu-id="51f29-258">Publish hello web application tooAzure and test it</span></span>
<span data-ttu-id="51f29-259">Nyní, budete publikovat vaše aplikace tooyour webové aplikace App Service a otestovat ji toosee, jak jste nakonfigurovali dříve hello hybridní připojení je právě používané tooconnect databáze toohello webové aplikace na místním počítači.</span><span class="sxs-lookup"><span data-stu-id="51f29-259">Now, you'll publish your application tooyour App Service web app and then test it toosee how hello hybrid connection you configured earlier is being used tooconnect your web app toohello database on your local machine.</span></span>

### <a name="publish-hello-web-application"></a><span data-ttu-id="51f29-260">Publikování webové aplikace hello</span><span class="sxs-lookup"><span data-stu-id="51f29-260">Publish hello web application</span></span>
1. <span data-ttu-id="51f29-261">Můžete si stáhnout váš profil publikování pro hello webové aplikace App Service v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="51f29-261">You can download your publishing profile for hello App Service web app in hello Azure Portal.</span></span> <span data-ttu-id="51f29-262">V okně hello pro vaši webovou aplikaci, klikněte na tlačítko **profilu publikování Get**a potom uložte hello souboru tooyour počítače.</span><span class="sxs-lookup"><span data-stu-id="51f29-262">On hello blade for your web app, click **Get publish profile**, and then save hello file tooyour computer.</span></span>
   
    ![Stažení profilu publikování][PortalDownloadPublishProfile]
   
    <span data-ttu-id="51f29-264">Dále budete importovat tento soubor do webové aplikace Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="51f29-264">Next, you will import this file into your Visual Studio web application.</span></span>
2. <span data-ttu-id="51f29-265">V sadě Visual Studio, klikněte pravým tlačítkem na název projektu hello v Průzkumníku řešení a vyberte **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="51f29-265">In Visual Studio, right-click hello project name in Solution Explorer and select **Publish**.</span></span>
   
    ![Vyberte publikování][HCVSRightClickProjectSelectPublish]
3. <span data-ttu-id="51f29-267">V hello **Publikovat Web** dialog v hello **profil** , zvolte **Import**.</span><span class="sxs-lookup"><span data-stu-id="51f29-267">In hello **Publish Web** dialog, on hello **Profile** tab, choose **Import**.</span></span>
   
    ![Import][HCVSPublishWebDialogImport]
4. <span data-ttu-id="51f29-269">Procházet tooyour stáhnout profil publikování, vyberte ho a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="51f29-269">Browse tooyour downloaded publishing profile, select it, and then click **OK**.</span></span>
   
    ![Procházet tooprofile][HCVSBrowseToImportPubProfile]
5. <span data-ttu-id="51f29-271">Informace o publikování je naimportována a zobrazí v hello **připojení** kartě hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="51f29-271">Your publishing information is imported and displays on hello **Connection** tab of hello dialog.</span></span>
   
    ![Klikněte na tlačítko Publikovat][HCVSClickPublish]
   
    <span data-ttu-id="51f29-273">Klikněte na **Publikovat**.</span><span class="sxs-lookup"><span data-stu-id="51f29-273">Click **Publish**.</span></span>
   
    <span data-ttu-id="51f29-274">Po dokončení publikování bude váš prohlížeč spustit a zobrazit aplikace teď známé ASP.NET – s tím rozdílem, že je nyní za provozu v hello cloudu Azure!</span><span class="sxs-lookup"><span data-stu-id="51f29-274">When publishing completes, your browser will launch and show your now familiar ASP.NET application -- except that now it is live in hello Azure cloud!</span></span>

<span data-ttu-id="51f29-275">V dalším kroku použijete vaše živou webovou aplikaci toosee jeho hybridní připojení v akci.</span><span class="sxs-lookup"><span data-stu-id="51f29-275">Next, you will use your live web application toosee its Hybrid Connection in action.</span></span>

### <a name="test-hello-completed-web-application-on-azure"></a><span data-ttu-id="51f29-276">Test hello dokončit webové aplikace v Azure</span><span class="sxs-lookup"><span data-stu-id="51f29-276">Test hello completed web application on Azure</span></span>
1. <span data-ttu-id="51f29-277">V horní části hello napravo od webové stránky v Azure, zvolte **přihlásit**.</span><span class="sxs-lookup"><span data-stu-id="51f29-277">On hello top right of your web page on Azure, choose **Log in**.</span></span>
   
    ![Test přihlášení][HCTestLogIn]
2. <span data-ttu-id="51f29-279">App Service, které se webová aplikace je teď připojené databáze členství tooyour webové aplikace na místním počítači.</span><span class="sxs-lookup"><span data-stu-id="51f29-279">Your App Service web app is now connected tooyour web application's membership database on your local machine.</span></span> <span data-ttu-id="51f29-280">tooverify se přihlaste se pomocí stejných přihlašovacích údajů, které jste zadali v hello místní databáze dříve hello.</span><span class="sxs-lookup"><span data-stu-id="51f29-280">tooverify this, log in with hello same credentials that you entered in hello local database earlier.</span></span>
   
    ![Hello pozdravu][HCTestHelloContoso]
3. <span data-ttu-id="51f29-282">toofurther vyzkoušejte nové hybridní připojení, odhlaste se z Azure webové aplikace a zaregistrujte se jako jiný uživatel.</span><span class="sxs-lookup"><span data-stu-id="51f29-282">toofurther test your new hybrid connection, log off of your Azure web application and register as another user.</span></span> <span data-ttu-id="51f29-283">Zadejte nové uživatelské jméno a heslo a potom klikněte na **zaregistrovat**.</span><span class="sxs-lookup"><span data-stu-id="51f29-283">Provide a new user name and password, and then click **Register**.</span></span>
   
    ![Test zaregistrovat jiný uživatel][HCTestRegisterRelecloud]
4. <span data-ttu-id="51f29-285">tooverify že hello přihlašovací údaje nového uživatele byly uloženy v místní databázi prostřednictvím hybridní připojení, otevřete SQL Management Studio v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="51f29-285">tooverify that hello new user's credentials have been stored in your local database through your hybrid connection, open SQL Management Studio on your local computer.</span></span> <span data-ttu-id="51f29-286">V Průzkumníku objektů rozbalte hello **MembershipDB** databáze a potom rozbalte **tabulky**.</span><span class="sxs-lookup"><span data-stu-id="51f29-286">In Object Explorer, expand hello **MembershipDB** database, and then expand **Tables**.</span></span> <span data-ttu-id="51f29-287">Klikněte pravým tlačítkem na hello **dbo. AspNetUsers** členství tabulky a zvolte **řádky vyberte Top 1000** tooview hello výsledky.</span><span class="sxs-lookup"><span data-stu-id="51f29-287">Right-click hello **dbo.AspNetUsers** membership table and choose **Select Top 1000 Rows** tooview hello results.</span></span>
   
    ![Zobrazení výsledků hello][HCTestSSMSTree]
5. <span data-ttu-id="51f29-289">Vaše tabulka členství v místní nyní zobrazuje oba účty - hello ten, který jste vytvořili místně a hello ten, který jste vytvořili v hello cloudu Azure.</span><span class="sxs-lookup"><span data-stu-id="51f29-289">Your local membership table now shows both accounts - hello one that you created locally, and hello one that you created in hello Azure cloud.</span></span> <span data-ttu-id="51f29-290">Hello ten, který jste vytvořili v cloudu hello se uložila tooyour místní databázi pomocí funkce Azure hybridní připojení.</span><span class="sxs-lookup"><span data-stu-id="51f29-290">hello one that you created in hello cloud has been saved tooyour on-premises database through Azure's Hybrid Connection feature.</span></span>
   
    ![Registrovaní uživatelé v místní databázi][HCTestShowMemberDb]

<span data-ttu-id="51f29-292">Nyní jste vytvořili a nasazené webové aplikace ASP.NET, která používá hybridní připojení mezi webovou aplikaci v hello cloudu Azure a místní databázi systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="51f29-292">You have now created and deployed an ASP.NET web application that uses a hybrid connection between a web app in hello Azure cloud and an on-premises SQL Server database.</span></span> <span data-ttu-id="51f29-293">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="51f29-293">Congratulations!</span></span>

## <a name="see-also"></a><span data-ttu-id="51f29-294">Viz také</span><span class="sxs-lookup"><span data-stu-id="51f29-294">See Also</span></span>
[<span data-ttu-id="51f29-295">Přehled hybridních připojení</span><span class="sxs-lookup"><span data-stu-id="51f29-295">Hybrid Connections overview</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=397274)

[<span data-ttu-id="51f29-296">Josh Točitost zavádí hybridní připojení (video na kanálu 9)</span><span class="sxs-lookup"><span data-stu-id="51f29-296">Josh Twist introduces hybrid connections (Channel 9 video)</span></span>](http://channel9.msdn.com/Shows/Azure-Friday/Josh-Twist-introduces-hybrid-connections)

[<span data-ttu-id="51f29-297">Přehled hybridních připojení</span><span class="sxs-lookup"><span data-stu-id="51f29-297">Hybrid Connections overview</span></span>](/services/biztalk-services/)

[<span data-ttu-id="51f29-298">BizTalk Services: Karty řídicí panel, sledování, škálování, konfigurovat a hybridní připojení.</span><span class="sxs-lookup"><span data-stu-id="51f29-298">BizTalk Services: Dashboard, Monitor, Scale, Configure, and Hybrid Connection tabs</span></span>](../biztalk-services/biztalk-dashboard-monitor-scale-tabs.md)

[<span data-ttu-id="51f29-299">Vytváření reálného hybridního cloudu s bezproblémové přenositelnost aplikace (Channel 9 video)</span><span class="sxs-lookup"><span data-stu-id="51f29-299">Building a Real-World Hybrid Cloud with Seamless Application Portability (Channel 9 video)</span></span>](http://channel9.msdn.com/events/TechEd/NorthAmerica/2014/DCIM-B323#fbid=)

[<span data-ttu-id="51f29-300">Přístup k místním prostředkům v Azure App Service pomocí hybridních připojení</span><span class="sxs-lookup"><span data-stu-id="51f29-300">Access on-premises resources using hybrid connections in Azure App Service</span></span>](web-sites-hybrid-connection-get-started.md)

[<span data-ttu-id="51f29-301">Přehled technologie ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="51f29-301">ASP.NET Identity Overview</span></span>](http://www.asp.net/identity)

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
