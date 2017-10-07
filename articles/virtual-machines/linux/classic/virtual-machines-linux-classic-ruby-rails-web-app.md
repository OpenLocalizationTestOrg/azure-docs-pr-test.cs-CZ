---
title: "aaaHost Ruby, na které webu na virtuální počítač s Linuxem | Microsoft Docs"
description: "Nastavení a hostitelem Ruby na web na základě které v Azure pomocí virtuální počítač s Linuxem."
services: virtual-machines-linux
documentationcenter: ruby
author: rmcmurray
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: aad32685-3550-4bff-9c73-beb8d70b3291
ms.service: virtual-machines-linux
ms.workload: web
ms.tgt_pltfrm: vm-linux
ms.devlang: ruby
ms.topic: article
ms.date: 06/27/2017
ms.author: robmcm
ms.openlocfilehash: c545c24fc6c89497854bbe55a6d0d1d0b072c386
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="ruby-on-rails-web-application-on-an-azure-vm"></a><span data-ttu-id="9080e-103">Webová aplikace Ruby on Rails na virtuálním počítači Azure</span><span class="sxs-lookup"><span data-stu-id="9080e-103">Ruby on Rails Web application on an Azure VM</span></span>
<span data-ttu-id="9080e-104">Tento kurz ukazuje, jak toohost Ruby, na které webu v Azure pomocí virtuální počítač s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="9080e-104">This tutorial shows how toohost a Ruby on Rails website on Azure using a Linux virtual machine.</span></span>  

<span data-ttu-id="9080e-105">V tomto kurzu se ověřila pomocí Ubuntu Server 14.04 LTS.</span><span class="sxs-lookup"><span data-stu-id="9080e-105">This tutorial was validated using Ubuntu Server 14.04 LTS.</span></span> <span data-ttu-id="9080e-106">Pokud používáte jiný distribuční Linux, bude pravděpodobně nutné toomodify hello kroky, které tooinstall.</span><span class="sxs-lookup"><span data-stu-id="9080e-106">If you use a different Linux distribution, you might need toomodify hello steps tooinstall Rails.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="9080e-107">Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../../../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="9080e-107">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../../../azure-resource-manager/resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="9080e-108">Tento článek se zabývá pomocí modelu nasazení classic hello.</span><span class="sxs-lookup"><span data-stu-id="9080e-108">This article covers using hello classic deployment model.</span></span> <span data-ttu-id="9080e-109">Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="9080e-109">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span>
>
>

## <a name="create-an-azure-vm"></a><span data-ttu-id="9080e-110">Vytvoření virtuálního počítače Azure</span><span class="sxs-lookup"><span data-stu-id="9080e-110">Create an Azure VM</span></span>
<span data-ttu-id="9080e-111">Začněte vytvořením virtuálního počítače Azure s bitovou kopii systému Linux.</span><span class="sxs-lookup"><span data-stu-id="9080e-111">Start by creating an Azure VM with a Linux image.</span></span>

<span data-ttu-id="9080e-112">toocreate hello virtuálních počítačů, můžete použít hello portál Azure nebo hello rozhraní příkazového řádku Azure (CLI).</span><span class="sxs-lookup"><span data-stu-id="9080e-112">toocreate hello VM, you can use hello Azure portal or hello Azure Command-Line Interface (CLI).</span></span>

### <a name="azure-portal"></a><span data-ttu-id="9080e-113">portál Azure</span><span class="sxs-lookup"><span data-stu-id="9080e-113">Azure portal</span></span>
1. <span data-ttu-id="9080e-114">Přihlaste se k hello [portálu Azure](https://portal.azure.com)</span><span class="sxs-lookup"><span data-stu-id="9080e-114">Sign into hello [Azure portal](https://portal.azure.com)</span></span>
2. <span data-ttu-id="9080e-115">Klikněte na tlačítko **nový**, hello vyhledávacího pole zadejte "Ubuntu Server 14.04".</span><span class="sxs-lookup"><span data-stu-id="9080e-115">Click **New**, then type "Ubuntu Server 14.04" in hello search box.</span></span> <span data-ttu-id="9080e-116">Klikněte na položku hello vrácený hello vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="9080e-116">Click hello entry returned by hello search.</span></span> <span data-ttu-id="9080e-117">Pro model nasazení hello, vyberte **Classic**, pak klikněte na tlačítko "Vytvořit".</span><span class="sxs-lookup"><span data-stu-id="9080e-117">For hello deployment model, select **Classic**, then click "Create".</span></span>
3. <span data-ttu-id="9080e-118">V okně základy hello, zadejte hodnoty pro pole hello požadované: název (pro hello virtuálních počítačů), uživatelské jméno, typ ověřování a přihlašovací údaje odpovídající hello, předplatné, skupinu prostředků a umístění.</span><span class="sxs-lookup"><span data-stu-id="9080e-118">In hello Basics blade, supply values for hello required fields: Name (for hello VM), User name, Authentication type and hello corresponding credentials, Azure subscription, Resource group, and Location.</span></span>

   ![Vytvořit novou bitovou kopii Ubuntu](./media/virtual-machines-linux-classic-ruby-rails-web-app/createvm.png)

4. <span data-ttu-id="9080e-120">Po zřízení hello virtuálních počítačů, klikněte na název virtuálního počítače hello a klikněte na tlačítko **koncové body** v hello **nastavení** kategorie.</span><span class="sxs-lookup"><span data-stu-id="9080e-120">After hello VM is provisioned, click on hello VM name, and click **Endpoints** in hello **Settings** category.</span></span> <span data-ttu-id="9080e-121">Najít hello SSH koncový bod, uvedené v části **samostatné**.</span><span class="sxs-lookup"><span data-stu-id="9080e-121">Find hello SSH endpoint, listed under **Standalone**.</span></span>

   ![Výchozí koncový bod](./media/virtual-machines-linux-classic-ruby-rails-web-app/endpointsnewportal.png)

### <a name="azure-cli"></a><span data-ttu-id="9080e-123">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="9080e-123">Azure CLI</span></span>
<span data-ttu-id="9080e-124">Postupujte podle kroků hello v [vytvořit virtuálním počítači systémem Linux][vm-instructions].</span><span class="sxs-lookup"><span data-stu-id="9080e-124">Follow hello steps in [Create a Virtual Machine Running Linux][vm-instructions].</span></span>

<span data-ttu-id="9080e-125">Po zřízení hello virtuálních počítačů můžete získat koncový bod SSH hello spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="9080e-125">After hello VM is provisioned, you can get hello SSH endpoint by running hello following command:</span></span>

    azure vm endpoint list <vm-name>  

## <a name="install-ruby-on-rails"></a><span data-ttu-id="9080e-126">Nainstalujte na které Ruby</span><span class="sxs-lookup"><span data-stu-id="9080e-126">Install Ruby on Rails</span></span>
1. <span data-ttu-id="9080e-127">Pomocí SSH tooconnect toohello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="9080e-127">Use SSH tooconnect toohello VM.</span></span>
2. <span data-ttu-id="9080e-128">Z relace SSH hello použijte následující příkazy tooinstall Ruby na hello virtuálních počítačů hello:</span><span class="sxs-lookup"><span data-stu-id="9080e-128">From hello SSH session, use hello following commands tooinstall Ruby on hello VM:</span></span>

        sudo apt-get update -y
        sudo apt-get upgrade -y

        sudo apt-add-repository ppa:brightbox/ruby-ng
        sudo apt-get update
        sudo apt-get install ruby2.4

        > [!TIP]
        > hello brightbox repository contains hello current Ruby distribution.

    <span data-ttu-id="9080e-129">Hello instalace může trvat několik minut.</span><span class="sxs-lookup"><span data-stu-id="9080e-129">hello installation may take a few minutes.</span></span> <span data-ttu-id="9080e-130">Po dokončení, použijte následující příkaz tooverify nainstalovanou Ruby hello:</span><span class="sxs-lookup"><span data-stu-id="9080e-130">When it completes, use hello following command tooverify that Ruby is installed:</span></span>

        ruby -v

3. <span data-ttu-id="9080e-131">Použití hello následující příkaz tooinstall které:</span><span class="sxs-lookup"><span data-stu-id="9080e-131">Use hello following command tooinstall Rails:</span></span>

        sudo gem install rails --no-rdoc --no-ri -V

    <span data-ttu-id="9080e-132">Použijte hello – ne rdoc a--ne ri příznaky tooskip instalaci hello dokumentace, která je rychlejší.</span><span class="sxs-lookup"><span data-stu-id="9080e-132">Use hello --no-rdoc and --no-ri flags tooskip installing hello documentation, which is faster.</span></span>
    <span data-ttu-id="9080e-133">Tento příkaz bude pravděpodobně trvat dlouhou dobu tooexecute, takže přidání hello -V se zobrazí informace o průběhu instalace hello.</span><span class="sxs-lookup"><span data-stu-id="9080e-133">This command will likely take a long time tooexecute, so adding hello -V will display information about hello installation progress.</span></span>

## <a name="create-and-run-an-app"></a><span data-ttu-id="9080e-134">Vytvoření a spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="9080e-134">Create and run an app</span></span>
<span data-ttu-id="9080e-135">Při přihlášení se stále pomocí protokolu SSH, spusťte následující příkazy hello:</span><span class="sxs-lookup"><span data-stu-id="9080e-135">While still logged in via SSH, run hello following commands:</span></span>

    rails new myapp
    cd myapp
    rails server -b 0.0.0.0 -p 3000

<span data-ttu-id="9080e-136">Hello [nové](http://guides.rubyonrails.org/command_line.html#rails-new) příkaz vytvoří novou aplikaci které.</span><span class="sxs-lookup"><span data-stu-id="9080e-136">hello [new](http://guides.rubyonrails.org/command_line.html#rails-new) command creates a new Rails app.</span></span> <span data-ttu-id="9080e-137">Hello [server](http://guides.rubyonrails.org/command_line.html#rails-server) příkaz spustí hello WEBrick webový server, která se dodává s které.</span><span class="sxs-lookup"><span data-stu-id="9080e-137">hello [server](http://guides.rubyonrails.org/command_line.html#rails-server) command starts hello WEBrick web server that comes with Rails.</span></span> <span data-ttu-id="9080e-138">(Pro použití v provozním prostředí, by pravděpodobně chcete toouse jiný server, jako je například Unicorn nebo osobní.)</span><span class="sxs-lookup"><span data-stu-id="9080e-138">(For production use, you would probably want toouse a different server, such as Unicorn or Passenger.)</span></span>

<span data-ttu-id="9080e-139">Měli byste vidět výstup podobný toohello následující.</span><span class="sxs-lookup"><span data-stu-id="9080e-139">You should see output similar toohello following.</span></span>

    => Booting WEBrick
    => Rails 4.2.1 application starting in development on http://0.0.0.0:3000
    => Run `rails server -h` for more startup options
    => Ctrl-C tooshutdown server
    [2015-06-09 23:34:23] INFO  WEBrick 1.3.1
    [2015-06-09 23:34:23] INFO  ruby 1.9.3 (2013-11-22) [x86_64-linux]
    [2015-06-09 23:34:23] INFO  WEBrick::HTTPServer#start: pid=27766 port=3000

## <a name="add-an-endpoint"></a><span data-ttu-id="9080e-140">Přidání koncového bodu</span><span class="sxs-lookup"><span data-stu-id="9080e-140">Add an endpoint</span></span>
1. <span data-ttu-id="9080e-141">Přejděte toohello [portál Azure] [https://portal.azure.com] a vyberte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="9080e-141">Go toohello [Azure portal][https://portal.azure.com] and select your VM.</span></span>

2. <span data-ttu-id="9080e-142">Vyberte **koncové body** v hello **nastavení** podél stránku hello hello levý okraj.</span><span class="sxs-lookup"><span data-stu-id="9080e-142">Select **ENDPOINTS** in hello **Settings** along hello left edge hello page.</span></span>

3. <span data-ttu-id="9080e-143">Klikněte na tlačítko **přidat** hello horní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="9080e-143">Click **ADD** at hello top of hello page.</span></span>

4. <span data-ttu-id="9080e-144">V hello **přidání koncového bodu** dialogovém okně zadejte hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="9080e-144">In hello **Add endpoint** dialog page, enter hello following information:</span></span>

   * <span data-ttu-id="9080e-145">**Název**: HTTP</span><span class="sxs-lookup"><span data-stu-id="9080e-145">**Name**: HTTP</span></span>
   * <span data-ttu-id="9080e-146">**Protokol**: TCP</span><span class="sxs-lookup"><span data-stu-id="9080e-146">**Protocol**: TCP</span></span>
   * <span data-ttu-id="9080e-147">**Veřejný port**: 80</span><span class="sxs-lookup"><span data-stu-id="9080e-147">**Public port**: 80</span></span>
   * <span data-ttu-id="9080e-148">**Privátní port**: 3000</span><span class="sxs-lookup"><span data-stu-id="9080e-148">**Private port**: 3000</span></span>
   * <span data-ttu-id="9080e-149">**Plovoucí PÍ adresu**: zakázáno</span><span class="sxs-lookup"><span data-stu-id="9080e-149">**Floating PI address**: Disabled</span></span>
   * <span data-ttu-id="9080e-150">**Seznam řízení přístupu – pořadí**: 1001 nebo jinou hodnotu, která nastavuje prioritu hello toto přístupové pravidlo.</span><span class="sxs-lookup"><span data-stu-id="9080e-150">**Access control list - Order**: 1001, or another value that sets hello priority of this access rule.</span></span>
   * <span data-ttu-id="9080e-151">**Seznam řízení přístupu – název**: allowHTTP</span><span class="sxs-lookup"><span data-stu-id="9080e-151">**Access control list - Name**: allowHTTP</span></span>
   * <span data-ttu-id="9080e-152">**Seznam řízení přístupu – akce**: Povolit</span><span class="sxs-lookup"><span data-stu-id="9080e-152">**Access control list - Action**: permit</span></span>
   * <span data-ttu-id="9080e-153">**Seznam řízení přístupu – vzdálené podsíti**: 1.0.0.0/16</span><span class="sxs-lookup"><span data-stu-id="9080e-153">**Access control list - Remote subnet**: 1.0.0.0/16</span></span>

     <span data-ttu-id="9080e-154">Tento koncový bod má veřejný port 80, který bude směrovat provoz toohello privátní port 3000, kde hello které server naslouchá.</span><span class="sxs-lookup"><span data-stu-id="9080e-154">This endpoint  has a public port of 80 that will route traffic toohello private port of 3000, where hello Rails server is listening.</span></span> <span data-ttu-id="9080e-155">Hello pravidlo seznamu řízení přístupu umožňuje veřejné přenosy na portu 80.</span><span class="sxs-lookup"><span data-stu-id="9080e-155">hello access control list rule allows public traffic on port 80.</span></span>

     ![Nový koncový bod](./media/virtual-machines-linux-classic-ruby-rails-web-app/createendpoint.png)

5. <span data-ttu-id="9080e-157">Klikněte na tlačítko OK toosave hello koncový bod.</span><span class="sxs-lookup"><span data-stu-id="9080e-157">Click OK toosave hello endpoint.</span></span>

6. <span data-ttu-id="9080e-158">Měli zobrazí zpráva s oznámením **ukládání koncový bod virtuálního počítače**.</span><span class="sxs-lookup"><span data-stu-id="9080e-158">A message should appear that states **Saving virtual machine endpoint**.</span></span> <span data-ttu-id="9080e-159">Jakmile se tato zpráva zmizí, koncový bod hello je aktivní.</span><span class="sxs-lookup"><span data-stu-id="9080e-159">Once this message disappears, hello endpoint is active.</span></span> <span data-ttu-id="9080e-160">Teď může otestovat aplikaci tak, že přejdete toohello název DNS virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="9080e-160">You may now test your application by navigating toohello DNS name of your virtual machine.</span></span> <span data-ttu-id="9080e-161">Hello webu by měla vypadat podobně jako toohello následující:</span><span class="sxs-lookup"><span data-stu-id="9080e-161">hello website should appear similar toohello following:</span></span>

    ![Výchozí stránka které][default-rails-cloud]

## <a name="next-steps"></a><span data-ttu-id="9080e-163">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9080e-163">Next steps</span></span>
<span data-ttu-id="9080e-164">V tomto kurzu jste to udělali většinu hello kroky ručně.</span><span class="sxs-lookup"><span data-stu-id="9080e-164">In this tutorial, you did most of hello steps manually.</span></span> <span data-ttu-id="9080e-165">V produkčním prostředí by zapsat aplikaci na vývojovém počítači a nasaďte ji toohello virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="9080e-165">In a production environment, you would write your app on a development machine and deploy it toohello Azure VM.</span></span> <span data-ttu-id="9080e-166">Navíc většině produkčních prostředích hostování aplikace, které hello ve spojení s jiným procesem serveru, jako je například Apache či NginX, která zpracovává žádosti směrování toomultiple instance hello které aplikace a obsluhuje statické prostředky.</span><span class="sxs-lookup"><span data-stu-id="9080e-166">Also, most production environments host hello Rails application in conjunction with another server process such as Apache or NginX, which handles request routing toomultiple instances of hello Rails application and serving static resources.</span></span> <span data-ttu-id="9080e-167">Další informace najdete v tématu http://rubyonrails.org/deploy/.</span><span class="sxs-lookup"><span data-stu-id="9080e-167">For more information, see http://rubyonrails.org/deploy/.</span></span>

<span data-ttu-id="9080e-168">toolearn Další informace o Ruby, na které, navštivte hello [Ruby, na které příručky][rails-guides].</span><span class="sxs-lookup"><span data-stu-id="9080e-168">toolearn more about Ruby on Rails, visit hello [Ruby on Rails Guides][rails-guides].</span></span>

<span data-ttu-id="9080e-169">toouse Azure služeb z Ruby aplikace naleznete v tématu:</span><span class="sxs-lookup"><span data-stu-id="9080e-169">toouse Azure services from your Ruby application, see:</span></span>

* <span data-ttu-id="9080e-170">[Ukládání nestrukturovaných dat pomocí objektů BLOB][blobs]</span><span class="sxs-lookup"><span data-stu-id="9080e-170">[Store unstructured data using blobs][blobs]</span></span>
* <span data-ttu-id="9080e-171">[Páry klíč – hodnota úložiště pomocí tabulky][tables]</span><span class="sxs-lookup"><span data-stu-id="9080e-171">[Store key/value pairs using tables][tables]</span></span>
* <span data-ttu-id="9080e-172">[Poskytovat obsah velkou šířku pásma s hello sítě pro doručování obsahu][cdn-howto]</span><span class="sxs-lookup"><span data-stu-id="9080e-172">[Serve high bandwidth content with hello Content Delivery Network][cdn-howto]</span></span>

<!-- WA.com links -->
[blobs]:../../../storage/blobs/storage-ruby-how-to-use-blob-storage.md
[cdn-howto]:https://azure.microsoft.com/develop/ruby/app-services/
[tables]:../../../cosmos-db/table-storage-how-to-use-ruby.md
[vm-instructions]:createportal.md

<!-- External Links -->
[rails-guides]:http://guides.rubyonrails.org/
[sqlite3]:http://www.sqlite.org/

<!-- Images -->

[default-rails-cloud]:./media/virtual-machines-linux-classic-ruby-rails-web-app/basicrailscloud.png
[vmlist]:./media/virtual-machines-linux-classic-ruby-rails-web-app/vmlist.png
[endpoints]:./media/virtual-machines-linux-classic-ruby-rails-web-app/endpoints.png
[new-endpoint]:./media/virtual-machines-linux-classic-ruby-rails-web-app/newendpoint.png
[new-endpoint1]:./media/virtual-machines-linux-classic-ruby-rails-web-app/newendpoint1.png
