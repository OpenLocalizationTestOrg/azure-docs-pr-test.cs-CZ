---
title: "Hostování Ruby, na které webu na virtuální počítač s Linuxem | Microsoft Docs"
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
ms.openlocfilehash: 0518519da6c5e62a863a47d6743ab7b7c5923acf
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="ruby-on-rails-web-application-on-an-azure-vm"></a><span data-ttu-id="f1750-103">Webová aplikace Ruby on Rails na virtuálním počítači Azure</span><span class="sxs-lookup"><span data-stu-id="f1750-103">Ruby on Rails Web application on an Azure VM</span></span>
<span data-ttu-id="f1750-104">Tento kurz ukazuje, jak k hostování Ruby, na které webu v Azure pomocí virtuální počítač s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="f1750-104">This tutorial shows how to host a Ruby on Rails website on Azure using a Linux virtual machine.</span></span>  

<span data-ttu-id="f1750-105">V tomto kurzu se ověřila pomocí Ubuntu Server 14.04 LTS.</span><span class="sxs-lookup"><span data-stu-id="f1750-105">This tutorial was validated using Ubuntu Server 14.04 LTS.</span></span> <span data-ttu-id="f1750-106">Pokud chcete použít jiný distribuční Linux, může musíte upravit postup instalace které.</span><span class="sxs-lookup"><span data-stu-id="f1750-106">If you use a different Linux distribution, you might need to modify the steps to install Rails.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f1750-107">Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../../../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="f1750-107">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../../../azure-resource-manager/resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="f1750-108">Tento článek se věnuje použití klasického modelu nasazení.</span><span class="sxs-lookup"><span data-stu-id="f1750-108">This article covers using the classic deployment model.</span></span> <span data-ttu-id="f1750-109">Microsoft doporučuje, aby byl ve většině nových nasazení použit model Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="f1750-109">Microsoft recommends that most new deployments use the Resource Manager model.</span></span>
>
>

## <a name="create-an-azure-vm"></a><span data-ttu-id="f1750-110">Vytvoření virtuálního počítače Azure</span><span class="sxs-lookup"><span data-stu-id="f1750-110">Create an Azure VM</span></span>
<span data-ttu-id="f1750-111">Začněte vytvořením virtuálního počítače Azure s bitovou kopii systému Linux.</span><span class="sxs-lookup"><span data-stu-id="f1750-111">Start by creating an Azure VM with a Linux image.</span></span>

<span data-ttu-id="f1750-112">Pokud chcete vytvořit virtuální počítač, můžete použít portál Azure nebo rozhraní příkazového řádku Azure (CLI).</span><span class="sxs-lookup"><span data-stu-id="f1750-112">To create the VM, you can use the Azure portal or the Azure Command-Line Interface (CLI).</span></span>

### <a name="azure-portal"></a><span data-ttu-id="f1750-113">portál Azure</span><span class="sxs-lookup"><span data-stu-id="f1750-113">Azure portal</span></span>
1. <span data-ttu-id="f1750-114">Přihlaste se k [portálu Azure](https://portal.azure.com)</span><span class="sxs-lookup"><span data-stu-id="f1750-114">Sign into the [Azure portal](https://portal.azure.com)</span></span>
2. <span data-ttu-id="f1750-115">Klikněte na tlačítko **nový**, zadejte do vyhledávacího pole "Ubuntu Server 14.04".</span><span class="sxs-lookup"><span data-stu-id="f1750-115">Click **New**, then type "Ubuntu Server 14.04" in the search box.</span></span> <span data-ttu-id="f1750-116">Klikněte na položku nalezené.</span><span class="sxs-lookup"><span data-stu-id="f1750-116">Click the entry returned by the search.</span></span> <span data-ttu-id="f1750-117">Pro model nasazení vyberte **Classic**, pak klikněte na tlačítko "Vytvořit".</span><span class="sxs-lookup"><span data-stu-id="f1750-117">For the deployment model, select **Classic**, then click "Create".</span></span>
3. <span data-ttu-id="f1750-118">V okně základy zadat hodnoty povinných polí: název (pro virtuální počítač), uživatelské jméno, typ ověřování a odpovídající údaje předplatného Azure, skupinu prostředků a umístění.</span><span class="sxs-lookup"><span data-stu-id="f1750-118">In the Basics blade, supply values for the required fields: Name (for the VM), User name, Authentication type and the corresponding credentials, Azure subscription, Resource group, and Location.</span></span>

   ![Vytvořit novou bitovou kopii Ubuntu](./media/virtual-machines-linux-classic-ruby-rails-web-app/createvm.png)

4. <span data-ttu-id="f1750-120">Po zřízení virtuálního počítače, klikněte na název virtuálního počítače a klikněte na tlačítko **koncové body** v **nastavení** kategorie.</span><span class="sxs-lookup"><span data-stu-id="f1750-120">After the VM is provisioned, click on the VM name, and click **Endpoints** in the **Settings** category.</span></span> <span data-ttu-id="f1750-121">Najít koncový bod SSH, uvedené v části **samostatné**.</span><span class="sxs-lookup"><span data-stu-id="f1750-121">Find the SSH endpoint, listed under **Standalone**.</span></span>

   ![Výchozí koncový bod](./media/virtual-machines-linux-classic-ruby-rails-web-app/endpointsnewportal.png)

### <a name="azure-cli"></a><span data-ttu-id="f1750-123">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="f1750-123">Azure CLI</span></span>
<span data-ttu-id="f1750-124">Postupujte podle kroků v [vytvořit virtuálním počítači systémem Linux][vm-instructions].</span><span class="sxs-lookup"><span data-stu-id="f1750-124">Follow the steps in [Create a Virtual Machine Running Linux][vm-instructions].</span></span>

<span data-ttu-id="f1750-125">Po zřízení virtuálního počítače, můžete získat koncový bod SSH spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="f1750-125">After the VM is provisioned, you can get the SSH endpoint by running the following command:</span></span>

    azure vm endpoint list <vm-name>  

## <a name="install-ruby-on-rails"></a><span data-ttu-id="f1750-126">Nainstalujte na které Ruby</span><span class="sxs-lookup"><span data-stu-id="f1750-126">Install Ruby on Rails</span></span>
1. <span data-ttu-id="f1750-127">Použití SSH se připojit k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="f1750-127">Use SSH to connect to the VM.</span></span>
2. <span data-ttu-id="f1750-128">Z relace SSH použijte následující příkazy pro instalaci Ruby ve virtuálním počítači:</span><span class="sxs-lookup"><span data-stu-id="f1750-128">From the SSH session, use the following commands to install Ruby on the VM:</span></span>

        sudo apt-get update -y
        sudo apt-get upgrade -y

        sudo apt-add-repository ppa:brightbox/ruby-ng
        sudo apt-get update
        sudo apt-get install ruby2.4

        > [!TIP]
        > The brightbox repository contains the current Ruby distribution.

    <span data-ttu-id="f1750-129">Instalace může trvat několik minut.</span><span class="sxs-lookup"><span data-stu-id="f1750-129">The installation may take a few minutes.</span></span> <span data-ttu-id="f1750-130">Po dokončení, ověřte, zda je nainstalován Ruby pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="f1750-130">When it completes, use the following command to verify that Ruby is installed:</span></span>

        ruby -v

3. <span data-ttu-id="f1750-131">Použijte následující příkaz k instalaci které:</span><span class="sxs-lookup"><span data-stu-id="f1750-131">Use the following command to install Rails:</span></span>

        sudo gem install rails --no-rdoc --no-ri -V

    <span data-ttu-id="f1750-132">Použití--ne rdoc a--ne ri příznaky tak, aby přeskočil instalace dokumentace, která je rychlejší.</span><span class="sxs-lookup"><span data-stu-id="f1750-132">Use the --no-rdoc and --no-ri flags to skip installing the documentation, which is faster.</span></span>
    <span data-ttu-id="f1750-133">Tento příkaz bude pravděpodobně trvat dlouhou dobu spuštění, takže přidání -V se zobrazí informace o průběhu instalace.</span><span class="sxs-lookup"><span data-stu-id="f1750-133">This command will likely take a long time to execute, so adding the -V will display information about the installation progress.</span></span>

## <a name="create-and-run-an-app"></a><span data-ttu-id="f1750-134">Vytvoření a spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="f1750-134">Create and run an app</span></span>
<span data-ttu-id="f1750-135">Při přihlášení se stále pomocí protokolu SSH, spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="f1750-135">While still logged in via SSH, run the following commands:</span></span>

    rails new myapp
    cd myapp
    rails server -b 0.0.0.0 -p 3000

<span data-ttu-id="f1750-136">[Nové](http://guides.rubyonrails.org/command_line.html#rails-new) příkaz vytvoří novou aplikaci které.</span><span class="sxs-lookup"><span data-stu-id="f1750-136">The [new](http://guides.rubyonrails.org/command_line.html#rails-new) command creates a new Rails app.</span></span> <span data-ttu-id="f1750-137">[Server](http://guides.rubyonrails.org/command_line.html#rails-server) příkaz spustí WEBrick webový server, který se dodává s které.</span><span class="sxs-lookup"><span data-stu-id="f1750-137">The [server](http://guides.rubyonrails.org/command_line.html#rails-server) command starts the WEBrick web server that comes with Rails.</span></span> <span data-ttu-id="f1750-138">(Pro použití v provozním prostředí, by pravděpodobně chcete použít jiný server, jako je například Unicorn nebo osobní.)</span><span class="sxs-lookup"><span data-stu-id="f1750-138">(For production use, you would probably want to use a different server, such as Unicorn or Passenger.)</span></span>

<span data-ttu-id="f1750-139">Zobrazený výstup by měl vypadat přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="f1750-139">You should see output similar to the following.</span></span>

    => Booting WEBrick
    => Rails 4.2.1 application starting in development on http://0.0.0.0:3000
    => Run `rails server -h` for more startup options
    => Ctrl-C to shutdown server
    [2015-06-09 23:34:23] INFO  WEBrick 1.3.1
    [2015-06-09 23:34:23] INFO  ruby 1.9.3 (2013-11-22) [x86_64-linux]
    [2015-06-09 23:34:23] INFO  WEBrick::HTTPServer#start: pid=27766 port=3000

## <a name="add-an-endpoint"></a><span data-ttu-id="f1750-140">Přidání koncového bodu</span><span class="sxs-lookup"><span data-stu-id="f1750-140">Add an endpoint</span></span>
1. <span data-ttu-id="f1750-141">Přejděte k [portálu Azure] [https://portal.azure.com] a vyberte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="f1750-141">Go to the [Azure portal][https://portal.azure.com] and select your VM.</span></span>

2. <span data-ttu-id="f1750-142">Vyberte **koncové body** v **nastavení** na levé straně okraj stránky.</span><span class="sxs-lookup"><span data-stu-id="f1750-142">Select **ENDPOINTS** in the **Settings** along the left edge the page.</span></span>

3. <span data-ttu-id="f1750-143">Klikněte na tlačítko **přidat** v horní části stránky.</span><span class="sxs-lookup"><span data-stu-id="f1750-143">Click **ADD** at the top of the page.</span></span>

4. <span data-ttu-id="f1750-144">V **přidání koncového bodu** dialogové okno stránky, zadejte následující informace:</span><span class="sxs-lookup"><span data-stu-id="f1750-144">In the **Add endpoint** dialog page, enter the following information:</span></span>

   * <span data-ttu-id="f1750-145">**Název**: HTTP</span><span class="sxs-lookup"><span data-stu-id="f1750-145">**Name**: HTTP</span></span>
   * <span data-ttu-id="f1750-146">**Protokol**: TCP</span><span class="sxs-lookup"><span data-stu-id="f1750-146">**Protocol**: TCP</span></span>
   * <span data-ttu-id="f1750-147">**Veřejný port**: 80</span><span class="sxs-lookup"><span data-stu-id="f1750-147">**Public port**: 80</span></span>
   * <span data-ttu-id="f1750-148">**Privátní port**: 3000</span><span class="sxs-lookup"><span data-stu-id="f1750-148">**Private port**: 3000</span></span>
   * <span data-ttu-id="f1750-149">**Plovoucí PÍ adresu**: zakázáno</span><span class="sxs-lookup"><span data-stu-id="f1750-149">**Floating PI address**: Disabled</span></span>
   * <span data-ttu-id="f1750-150">**Seznam řízení přístupu – pořadí**: 1001 nebo jinou hodnotu, která nastavuje prioritu Toto přístupové pravidlo.</span><span class="sxs-lookup"><span data-stu-id="f1750-150">**Access control list - Order**: 1001, or another value that sets the priority of this access rule.</span></span>
   * <span data-ttu-id="f1750-151">**Seznam řízení přístupu – název**: allowHTTP</span><span class="sxs-lookup"><span data-stu-id="f1750-151">**Access control list - Name**: allowHTTP</span></span>
   * <span data-ttu-id="f1750-152">**Seznam řízení přístupu – akce**: Povolit</span><span class="sxs-lookup"><span data-stu-id="f1750-152">**Access control list - Action**: permit</span></span>
   * <span data-ttu-id="f1750-153">**Seznam řízení přístupu – vzdálené podsíti**: 1.0.0.0/16</span><span class="sxs-lookup"><span data-stu-id="f1750-153">**Access control list - Remote subnet**: 1.0.0.0/16</span></span>

     <span data-ttu-id="f1750-154">Tento koncový bod má veřejný port 80, který bude směrovat provoz privátní port 3000, kde je server které přijímá.</span><span class="sxs-lookup"><span data-stu-id="f1750-154">This endpoint  has a public port of 80 that will route traffic to the private port of 3000, where the Rails server is listening.</span></span> <span data-ttu-id="f1750-155">Pravidlo seznamu řízení přístupu umožňuje veřejné přenosy na portu 80.</span><span class="sxs-lookup"><span data-stu-id="f1750-155">The access control list rule allows public traffic on port 80.</span></span>

     ![Nový koncový bod](./media/virtual-machines-linux-classic-ruby-rails-web-app/createendpoint.png)

5. <span data-ttu-id="f1750-157">Klikněte na tlačítko OK uložíte koncový bod.</span><span class="sxs-lookup"><span data-stu-id="f1750-157">Click OK to save the endpoint.</span></span>

6. <span data-ttu-id="f1750-158">Měli zobrazí zpráva s oznámením **ukládání koncový bod virtuálního počítače**.</span><span class="sxs-lookup"><span data-stu-id="f1750-158">A message should appear that states **Saving virtual machine endpoint**.</span></span> <span data-ttu-id="f1750-159">Jakmile se tato zpráva zmizí, koncový bod je aktivní.</span><span class="sxs-lookup"><span data-stu-id="f1750-159">Once this message disappears, the endpoint is active.</span></span> <span data-ttu-id="f1750-160">Teď může otestovat aplikaci tak, že přejdete na název DNS virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="f1750-160">You may now test your application by navigating to the DNS name of your virtual machine.</span></span> <span data-ttu-id="f1750-161">Web by měl vypadat přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="f1750-161">The website should appear similar to the following:</span></span>

    ![Výchozí stránka které][default-rails-cloud]

## <a name="next-steps"></a><span data-ttu-id="f1750-163">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f1750-163">Next steps</span></span>
<span data-ttu-id="f1750-164">V tomto kurzu jste to udělali většinu kroky ručně.</span><span class="sxs-lookup"><span data-stu-id="f1750-164">In this tutorial, you did most of the steps manually.</span></span> <span data-ttu-id="f1750-165">V produkčním prostředí by zapsat aplikaci na vývojovém počítači a nasazení virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="f1750-165">In a production environment, you would write your app on a development machine and deploy it to the Azure VM.</span></span> <span data-ttu-id="f1750-166">Většině produkčních prostředích hostovat které aplikace ve spojení s jiným procesem serveru, jako je například Apache či NginX, která zpracovává požadavků směrování na více instancí, které aplikace a obsluhuje statické prostředky.</span><span class="sxs-lookup"><span data-stu-id="f1750-166">Also, most production environments host the Rails application in conjunction with another server process such as Apache or NginX, which handles request routing to multiple instances of the Rails application and serving static resources.</span></span> <span data-ttu-id="f1750-167">Další informace najdete v tématu http://rubyonrails.org/deploy/.</span><span class="sxs-lookup"><span data-stu-id="f1750-167">For more information, see http://rubyonrails.org/deploy/.</span></span>

<span data-ttu-id="f1750-168">Další informace o Ruby, na které, najdete [Ruby, na které příručky][rails-guides].</span><span class="sxs-lookup"><span data-stu-id="f1750-168">To learn more about Ruby on Rails, visit the [Ruby on Rails Guides][rails-guides].</span></span>

<span data-ttu-id="f1750-169">Použití služeb Azure z Ruby aplikace naleznete v tématu:</span><span class="sxs-lookup"><span data-stu-id="f1750-169">To use Azure services from your Ruby application, see:</span></span>

* <span data-ttu-id="f1750-170">[Ukládání nestrukturovaných dat pomocí objektů BLOB][blobs]</span><span class="sxs-lookup"><span data-stu-id="f1750-170">[Store unstructured data using blobs][blobs]</span></span>
* <span data-ttu-id="f1750-171">[Páry klíč – hodnota úložiště pomocí tabulky][tables]</span><span class="sxs-lookup"><span data-stu-id="f1750-171">[Store key/value pairs using tables][tables]</span></span>
* <span data-ttu-id="f1750-172">[Poskytovat obsah velkou šířku pásma s sítě pro doručování obsahu][cdn-howto]</span><span class="sxs-lookup"><span data-stu-id="f1750-172">[Serve high bandwidth content with the Content Delivery Network][cdn-howto]</span></span>

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
