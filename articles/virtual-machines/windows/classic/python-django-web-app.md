---
title: "Webové aplikace Django na virtuální počítač Windows serveru Azure | Microsoft Docs"
description: "Zjistěte, jak k hostování webu na základě Django v Azure pomocí virtuální počítač s Windows Server 2012 R2 Datacenter s modelem nasazení classic."
services: virtual-machines-windows
documentationcenter: python
author: huguesv
manager: wpickett
editor: 
tags: azure-service-management
ms.assetid: e36484d1-afbf-47f5-b755-5e65397dc1c3
ms.service: virtual-machines-windows
ms.workload: web
ms.tgt_pltfrm: vm-windows
ms.devlang: python
ms.topic: article
ms.date: 05/31/2017
ms.author: huvalo
ms.openlocfilehash: 283a296fb39863c2801be1093cc4f56904786abd
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="django-hello-world-web-app-on-a-windows-server-vm"></a><span data-ttu-id="e3627-103">Django Hello World webové aplikace na virtuální počítač Windows serveru</span><span class="sxs-lookup"><span data-stu-id="e3627-103">Django Hello World web app on a Windows Server VM</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="e3627-104">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Azure Resource Manager a modelu nasazení classic](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="e3627-104">Azure has two different deployment models for creating and working with resources: [Azure Resource Manager and the classic deployment model](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="e3627-105">Tento článek popisuje model nasazení classic.</span><span class="sxs-lookup"><span data-stu-id="e3627-105">This article describes the classic deployment model.</span></span> <span data-ttu-id="e3627-106">Doporučujeme vám, že většina nových nasazení používala model Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="e3627-106">We recommend that most new deployments use the Resource Manager model.</span></span>

<span data-ttu-id="e3627-107">V tomto kurzu se dozvíte, jak k hostování webu na základě Django v systému Windows Server v Azure Virtual Machines.</span><span class="sxs-lookup"><span data-stu-id="e3627-107">This tutorial shows you how to host a Django-based website in Windows Server in Azure Virtual Machines.</span></span> <span data-ttu-id="e3627-108">V tomto kurzu předpokládáme žádné předchozí zkušenosti s Azure.</span><span class="sxs-lookup"><span data-stu-id="e3627-108">In the tutorial, we assume no prior experience with Azure.</span></span> <span data-ttu-id="e3627-109">Po dokončení tohoto kurzu, může mít jiné aplikace založené na rozhraní Django nahoru a běží v cloudu.</span><span class="sxs-lookup"><span data-stu-id="e3627-109">When you finish the tutorial, you can have a Django-based application up and running in the cloud.</span></span>

<span data-ttu-id="e3627-110">Naučte se:</span><span class="sxs-lookup"><span data-stu-id="e3627-110">Learn how to:</span></span>

* <span data-ttu-id="e3627-111">Nastavte virtuální počítač Azure na hostitele Django.</span><span class="sxs-lookup"><span data-stu-id="e3627-111">Set up an Azure virtual machine to host Django.</span></span> <span data-ttu-id="e3627-112">I když tento kurz vysvětluje, jak to udělat pro **systému Windows Server**, můžete provést stejný pro virtuální počítač Linux hostované v Azure.</span><span class="sxs-lookup"><span data-stu-id="e3627-112">Although this tutorial explains how to do this for **Windows Server**, you can do the same for a Linux VM hosted in Azure.</span></span>
* <span data-ttu-id="e3627-113">Vytvořte novou aplikaci Django v systému Windows.</span><span class="sxs-lookup"><span data-stu-id="e3627-113">Create a new Django application in Windows.</span></span>

<span data-ttu-id="e3627-114">Tento kurz ukazuje, jak pro vytvoření základní webové aplikace Hello World.</span><span class="sxs-lookup"><span data-stu-id="e3627-114">The tutorial shows you how to build a basic Hello World web application.</span></span> <span data-ttu-id="e3627-115">Aplikace je hostitelem virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="e3627-115">The application is hosted in an Azure virtual machine.</span></span>

<span data-ttu-id="e3627-116">Na následujícím snímku obrazovky je vidět hotová aplikace:</span><span class="sxs-lookup"><span data-stu-id="e3627-116">The following screenshot shows the completed application:</span></span>

![Okno prohlížeče zobrazí stránku hello world v Azure][1]

[!INCLUDE [create-account-and-vms-note](../../../../includes/create-account-and-vms-note.md)]

## <a name="create-and-set-up-an-azure-virtual-machine-to-host-django"></a><span data-ttu-id="e3627-118">Vytvoření a nastavení virtuálního počítače Azure na hostitele Django</span><span class="sxs-lookup"><span data-stu-id="e3627-118">Create and set up an Azure virtual machine to host Django</span></span>

1. <span data-ttu-id="e3627-119">Pokud chcete vytvořit virtuální počítač Azure s Windows Server 2012 R2 Datacenter distribuce, najdete v části [vytvoření virtuálního počítače se systémem Windows na portálu Azure](tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="e3627-119">To create an Azure virtual machine with the Windows Server 2012 R2 Datacenter distribution, see [Create a virtual machine running Windows in the Azure portal](tutorial.md).</span></span>
2. <span data-ttu-id="e3627-120">Nastavení pro přesměrování portu 80 provoz z webu na portu 80 pro virtuální počítač Azure:</span><span class="sxs-lookup"><span data-stu-id="e3627-120">Set Azure to direct port 80 traffic from the web to port 80 on the virtual machine:</span></span>
   
   1. <span data-ttu-id="e3627-121">Na portálu Azure přejděte do řídicího panelu a vyberte nově vytvořenou virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e3627-121">In the Azure portal, go to the dashboard and select your newly created virtual machine.</span></span>
   2. <span data-ttu-id="e3627-122">Klikněte na **Koncové body** a potom na **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="e3627-122">Click **Endpoints**, and then click **Add**.</span></span>

     ![Přidání koncového bodu](./media/python-django-web-app/django-helloworld-add-endpoint-new-portal.png)

   3. <span data-ttu-id="e3627-124">Na **přidání koncového bodu** stránky, pro **název**, zadejte **HTTP**.</span><span class="sxs-lookup"><span data-stu-id="e3627-124">On the **Add endpoint** page, for **Name**, enter **HTTP**.</span></span> <span data-ttu-id="e3627-125">Nastavte TCP veřejné a privátní porty na **80**.</span><span class="sxs-lookup"><span data-stu-id="e3627-125">Set the public and private TCP ports to **80**.</span></span>

     ![Zadejte název a nastavte veřejné a privátní porty](./media/python-django-web-app/django-helloworld-add-endpoint-set-ports-new-portal.png)

   4. <span data-ttu-id="e3627-127">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="e3627-127">Click **OK**.</span></span>
     
3. <span data-ttu-id="e3627-128">Na řídicím panelu vyberte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="e3627-128">In the dashboard, select your VM.</span></span> <span data-ttu-id="e3627-129">Chcete-li vzdáleně přihlašují k nově vytvořený virtuální počítač Azure pomocí protokolu RDP (Remote Desktop), klikněte na tlačítko **Connect**.</span><span class="sxs-lookup"><span data-stu-id="e3627-129">To use Remote Desktop Protocol (RDP) to remotely sign in to the newly created Azure virtual machine, click **Connect**.</span></span>  

> [!IMPORTANT] 
> <span data-ttu-id="e3627-130">Tyto pokyny předpokládají, že jste přihlášení k virtuálnímu počítači správně.</span><span class="sxs-lookup"><span data-stu-id="e3627-130">The following instructions assume that you signed in to the virtual machine correctly.</span></span> <span data-ttu-id="e3627-131">Také se předpokládá, že k vydání příkazů ve virtuálním počítači a není v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="e3627-131">They also assume that you are issuing commands in the virtual machine and not on your local computer.</span></span>

## <span data-ttu-id="e3627-132"><a id="setup"></a>Instalace Python, rozhraní Django a WFastCGI</span><span class="sxs-lookup"><span data-stu-id="e3627-132"><a id="setup"> </a>Install Python, Django, and WFastCGI</span></span>
> [!NOTE]
> <span data-ttu-id="e3627-133">Chcete-li stáhnout v aplikaci Internet Explorer, možná budete muset nakonfigurovat Internet Explorer **konfigurace rozšířeného zabezpečení aplikace** nastavení.</span><span class="sxs-lookup"><span data-stu-id="e3627-133">To download by using Internet Explorer, you might have to configure Internet Explorer **Enhanced Security Configuration** settings.</span></span> <span data-ttu-id="e3627-134">Chcete-li to provést, klikněte na tlačítko **spustit** > **nástroje pro správu** > **správce serveru** > **místní Server**.</span><span class="sxs-lookup"><span data-stu-id="e3627-134">To do this, click **Start** > **Administrative Tools** > **Server Manager** > **Local Server**.</span></span> <span data-ttu-id="e3627-135">Klikněte na tlačítko **konfigurace rozšířeného zabezpečení aplikace Internet Explorer**a potom vyberte **vypnout**.</span><span class="sxs-lookup"><span data-stu-id="e3627-135">Click **IE Enhanced Security Configuration**, and then select **Off**.</span></span>

1. <span data-ttu-id="e3627-136">Instalace nejnovější verze Python 2.7 nebo 3.4 Python z [python.org][python.org].</span><span class="sxs-lookup"><span data-stu-id="e3627-136">Install the latest versions of Python 2.7 or Python 3.4 from [python.org][python.org].</span></span>
2. <span data-ttu-id="e3627-137">Instalovat balíčky wfastcgi a django, pomocí nástroje pip.</span><span class="sxs-lookup"><span data-stu-id="e3627-137">Install the wfastcgi and django packages using pip.</span></span>
   
    <span data-ttu-id="e3627-138">Python 2.7 použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="e3627-138">For Python 2.7, use the following command:</span></span>
   
        c:\python27\scripts\pip install wfastcgi
        c:\python27\scripts\pip install django
   
    <span data-ttu-id="e3627-139">Pro Python 3.4 použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="e3627-139">For Python 3.4, use the following command:</span></span>
   
        c:\python34\scripts\pip install wfastcgi
        c:\python34\scripts\pip install django

## <a name="install-iis-with-fastcgi"></a><span data-ttu-id="e3627-140">Nainstalovat službu ISS s FastCGI</span><span class="sxs-lookup"><span data-stu-id="e3627-140">Install IIS with FastCGI</span></span>
* <span data-ttu-id="e3627-141">Instalace Internetové informační služby (IIS) s podporou rozhraní FastCGI.</span><span class="sxs-lookup"><span data-stu-id="e3627-141">Install Internet Information Services (IIS) with FastCGI support.</span></span> <span data-ttu-id="e3627-142">To může trvat několik minut provést.</span><span class="sxs-lookup"><span data-stu-id="e3627-142">This might take several minutes to execute.</span></span>
   
        start /wait %windir%\System32\PkgMgr.exe /iu:IIS-WebServerRole;IIS-WebServer;IIS-CommonHttpFeatures;IIS-StaticContent;IIS-DefaultDocument;IIS-DirectoryBrowsing;IIS-HttpErrors;IIS-HealthAndDiagnostics;IIS-HttpLogging;IIS-LoggingLibraries;IIS-RequestMonitor;IIS-Security;IIS-RequestFiltering;IIS-HttpCompressionStatic;IIS-WebServerManagementTools;IIS-ManagementConsole;WAS-WindowsActivationService;WAS-ProcessModel;WAS-NetFxEnvironment;WAS-ConfigurationAPI;IIS-CGI

## <a name="create-a-new-django-application"></a><span data-ttu-id="e3627-143">Vytvořte novou aplikaci Django</span><span class="sxs-lookup"><span data-stu-id="e3627-143">Create a new Django application</span></span>
1. <span data-ttu-id="e3627-144">V C:\inetpub\wwwroot Pokud chcete vytvořit nový projekt Django, zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="e3627-144">In C:\inetpub\wwwroot, to create a new Django project, enter the following command:</span></span>
   
   <span data-ttu-id="e3627-145">Python 2.7 použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="e3627-145">For Python 2.7, use the following command:</span></span>
   
       C:\Python27\Scripts\django-admin.exe startproject helloworld
   
   <span data-ttu-id="e3627-146">Pro Python 3.4 použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="e3627-146">For Python 3.4, use the following command:</span></span>
   
       C:\Python34\Scripts\django-admin.exe startproject helloworld
   
   ![Výsledek příkazu New-AzureService](./media/python-django-web-app/django-helloworld-cmd-new-azure-service.png)
2. <span data-ttu-id="e3627-148">`django-admin` Příkaz generuje základní strukturu pro weby založené na rozhraní Django:</span><span class="sxs-lookup"><span data-stu-id="e3627-148">The `django-admin` command generates a basic structure for Django-based websites:</span></span>
   
   * <span data-ttu-id="e3627-149">`helloworld\manage.py`umožňuje hostování spuštění a zastavení hostování svého webu na základě Django.</span><span class="sxs-lookup"><span data-stu-id="e3627-149">`helloworld\manage.py` helps you start hosting and stop hosting your Django-based website.</span></span>
   * <span data-ttu-id="e3627-150">`helloworld\helloworld\settings.py`má Django nastavení pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e3627-150">`helloworld\helloworld\settings.py` has Django settings for your application.</span></span>
   * <span data-ttu-id="e3627-151">`helloworld\helloworld\urls.py`má kód mapování mezi každou adresu URL a jeho zobrazení.</span><span class="sxs-lookup"><span data-stu-id="e3627-151">`helloworld\helloworld\urls.py` has the mapping code between each URL and its view.</span></span>
3. <span data-ttu-id="e3627-152">V adresáři C:\inetpub\wwwroot\helloworld\helloworld vytvořte nový soubor s názvem views.py.</span><span class="sxs-lookup"><span data-stu-id="e3627-152">In the C:\inetpub\wwwroot\helloworld\helloworld directory, create a new file named views.py.</span></span> <span data-ttu-id="e3627-153">Tento soubor obsahuje zobrazení, který vykreslí stránku "hello, world".</span><span class="sxs-lookup"><span data-stu-id="e3627-153">This file has the view that renders the "hello world" page.</span></span> <span data-ttu-id="e3627-154">V editoru kódu zadejte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="e3627-154">In your code editor, enter the following commands:</span></span>
   
       from django.http import HttpResponse
       def home(request):
           html = "<html><body>Hello World!</body></html>"
           return HttpResponse(html)
4. <span data-ttu-id="e3627-155">Nahraďte obsah souboru urls.py pomocí následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="e3627-155">Replace the contents of the urls.py file with the following commands:</span></span>
   
       from django.conf.urls import patterns, url
       urlpatterns = patterns('',
           url(r'^$', 'helloworld.views.home', name='home'),
       )

## <a name="set-up-iis"></a><span data-ttu-id="e3627-156">Nastavení služby IIS</span><span class="sxs-lookup"><span data-stu-id="e3627-156">Set up IIS</span></span>
1. <span data-ttu-id="e3627-157">V souboru applicationhost.config globální odemkněte oddíl obslužné rutiny.</span><span class="sxs-lookup"><span data-stu-id="e3627-157">In the global applicationhost.config file, unlock the handlers section.</span></span>  <span data-ttu-id="e3627-158">To umožňuje používat obslužná rutina Python souboru web.config.</span><span class="sxs-lookup"><span data-stu-id="e3627-158">This allows your web.config file to use the Python handler.</span></span> <span data-ttu-id="e3627-159">Přidejte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="e3627-159">Add this command:</span></span>
   
        %windir%\system32\inetsrv\appcmd unlock config -section:system.webServer/handlers
2. <span data-ttu-id="e3627-160">Aktivujte WFastCGI.</span><span class="sxs-lookup"><span data-stu-id="e3627-160">Activate WFastCGI.</span></span> <span data-ttu-id="e3627-161">Tento postup přidá aplikaci do souboru applicationhost.config globální, který odkazuje na vaše překladač Pythonu spustitelný soubor a wfastcgi.py skript.</span><span class="sxs-lookup"><span data-stu-id="e3627-161">This adds an application to the global applicationhost.config file, which refers to your Python interpreter executable and the wfastcgi.py script.</span></span>
   
    <span data-ttu-id="e3627-162">V Python 2.7:</span><span class="sxs-lookup"><span data-stu-id="e3627-162">In Python 2.7:</span></span>
   
        C:\python27\scripts\wfastcgi-enable
   
    <span data-ttu-id="e3627-163">V Pythonu 3.4:</span><span class="sxs-lookup"><span data-stu-id="e3627-163">In Python 3.4:</span></span>
   
        C:\python34\scripts\wfastcgi-enable
3. <span data-ttu-id="e3627-164">V C:\inetpub\wwwroot\helloworld vytvořte soubor web.config.</span><span class="sxs-lookup"><span data-stu-id="e3627-164">In C:\inetpub\wwwroot\helloworld, create a web.config file.</span></span> <span data-ttu-id="e3627-165">Hodnota `scriptProcessor` atribut by měl odpovídat výstup z předchozího kroku.</span><span class="sxs-lookup"><span data-stu-id="e3627-165">The value of the `scriptProcessor` attribute should match the output from the preceding step.</span></span> <span data-ttu-id="e3627-166">Další informace o nastavení wfastcgi najdete v tématu [úložiště pypi wfastcgi][wfastcgi].</span><span class="sxs-lookup"><span data-stu-id="e3627-166">For more information about the wfastcgi setting, see [pypi wfastcgi][wfastcgi].</span></span>
   
   <span data-ttu-id="e3627-167">V Python 2.7:</span><span class="sxs-lookup"><span data-stu-id="e3627-167">In  Python 2.7:</span></span>
   
        <configuration>
          <appSettings>
            <add key="WSGI_HANDLER" value="django.core.handlers.wsgi.WSGIHandler()" />
            <add key="PYTHONPATH" value="C:\inetpub\wwwroot\helloworld" />
            <add key="DJANGO_SETTINGS_MODULE" value="helloworld.settings" />
          </appSettings>
          <system.webServer>
            <handlers>
                <add name="Python FastCGI" path="*" verb="*" modules="FastCgiModule" scriptProcessor="C:\Python27\python.exe|C:\Python27\Lib\site-packages\wfastcgi.pyc" resourceType="Unspecified" />
            </handlers>
          </system.webServer>
        </configuration>
   
   <span data-ttu-id="e3627-168">V Pythonu 3.4:</span><span class="sxs-lookup"><span data-stu-id="e3627-168">In  Python 3.4:</span></span>
   
        <configuration>
          <appSettings>
            <add key="WSGI_HANDLER" value="django.core.handlers.wsgi.WSGIHandler()" />
            <add key="PYTHONPATH" value="C:\inetpub\wwwroot\helloworld" />
            <add key="DJANGO_SETTINGS_MODULE" value="helloworld.settings" />
          </appSettings>
          <system.webServer>
            <handlers>
                <add name="Python FastCGI" path="*" verb="*" modules="FastCgiModule" scriptProcessor="C:\Python34\python.exe|C:\Python34\Lib\site-packages\wfastcgi.py" resourceType="Unspecified" />
            </handlers>
          </system.webServer>
        </configuration>
4. <span data-ttu-id="e3627-169">Aktualizujte umístění na výchozí web služby IIS tak, aby odkazoval na složku projekt Django:</span><span class="sxs-lookup"><span data-stu-id="e3627-169">Update the location of the IIS default website to point to the Django project folder:</span></span>
   
        %windir%\system32\inetsrv\appcmd set vdir "Default Web Site/" -physicalPath:"C:\inetpub\wwwroot\helloworld"
5. <span data-ttu-id="e3627-170">Načtěte webovou stránku v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="e3627-170">Load the webpage in your browser.</span></span>

![Okno prohlížeče zobrazí stránku hello world v Azure][1]

## <a name="shut-down-your-azure-virtual-machine"></a><span data-ttu-id="e3627-172">Vypněte virtuální počítač Azure</span><span class="sxs-lookup"><span data-stu-id="e3627-172">Shut down your Azure virtual machine</span></span>
<span data-ttu-id="e3627-173">Po dokončení v tomto kurzu, doporučujeme vypnout nebo odeberte virtuální počítač Azure jste vytvořili pro tento kurz.</span><span class="sxs-lookup"><span data-stu-id="e3627-173">When you're done with this tutorial, we recommend that you shut down or remove the Azure VM you created for the tutorial.</span></span> <span data-ttu-id="e3627-174">Tím uvolní prostředky pro ostatní kurzy a účtovány poplatky za používání Azure.</span><span class="sxs-lookup"><span data-stu-id="e3627-174">This frees up resources for other tutorials, and you can avoid incurring Azure usage charges.</span></span>

[1]: ./media/python-django-web-app/django-helloworld-browser-azure.png

[port80]: ./media/python-django-web-app/django-helloworld-port80.png

[Web Platform Installer]: http://www.microsoft.com/web/downloads/platform.aspx
[python.org]: https://www.python.org/downloads/
[wfastcgi]: https://pypi.python.org/pypi/wfastcgi
