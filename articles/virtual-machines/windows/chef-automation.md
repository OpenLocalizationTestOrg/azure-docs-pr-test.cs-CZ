---
title: "nasazení virtuálního počítače aaaAzure s Chef | Microsoft Docs"
description: "Zjistěte, jak toouse Chef toodo automatizované nasazení virtuálního počítače a konfigurace v Microsoft Azure"
services: virtual-machines-windows
documentationcenter: 
author: diegoviso
manager: timlt
tags: azure-service-management,azure-resource-manager
editor: 
ms.assetid: 0b82ca70-89ed-496d-bb49-c04ae59b4523
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-multiple
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: diviso
ms.openlocfilehash: c5ea98c673b2ee75dd4cedf27e50330af05230d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="automating-azure-virtual-machine-deployment-with-chef"></a>Automatizace nasazení virtuálních počítačů Azure pomocí Chefu
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Chef je skvělý nástroj pro doručení automatizace a požadovaného stavu konfigurace.

Naše nejnovější verzi api cloudu Chef zajišťuje bezproblémovou integraci s Azure, budete hello možnost tooprovision a nasadit stavům konfigurace prostřednictvím jeden příkaz.

V tomto článku I vám ukáže, jak tooset až tooprovision prostředí vaší Chef Azure virtuální počítače, a provede vás vytvořením zásady nebo "Kuchařka" a pak nasazení této kuchařka tooan virtuální počítač Azure.

Můžeme začít!

## <a name="chef-basics"></a>Základy Chef
Než začnete, naznačují, že zkontrolujte hello základní koncepty Chef. Je skvělým materiálu <a href="http://www.chef.io/chef" target="_blank">zde</a> a I doporučujeme mít Rychlé čtení před provedením tohoto návodu. Základy hello I bude recap, ale než začneme.

Hello následující diagram znázorňuje Chef Architektura vysoké úrovně hello.

![][2]

Chef má třemi hlavními komponentami architektury: Chef, Chef klienta (uzel), Chef pracovní stanici a serveru.

Hello Chef serveru je naše bod správy a existují dvě možnosti pro hello Chef serveru: hostované řešení nebo v případě místních řešení. Použijeme hostované řešení.

Hello Chef klienta (uzel) je hello agenta, která se nachází na hello servery, které spravujete.

Hello Chef pracovní stanici je naše správce pracovní stanice, kde budeme vytvářet naše zásady a spustit příkazy pro správu. Spustíme hello **nůž** příkaz z hello pracovní stanice Chef toomanage naše infrastruktury.

Je také hello konceptu "Cookbooks" a "Recepty". Toto jsou efektivně hello zásady jsme definovat a použít tooour servery.

## <a name="preparing-hello-workstation"></a>Příprava pracovní stanice hello
Nejprve umožňuje přípravný hello pracovní stanice. Používám standardní pracovní stanice systému Windows. Naše konfigurační soubory a cookbooks potřebujeme toocreate toostore adresáře.

Nejprve vytvořte adresář s názvem C:\chef.

Potom vytvořte druhý adresář s názvem c:\chef\cookbooks.

Nyní potřebujeme toodownload naše souborů nastavení Azure, Chef může komunikovat s naše předplatné Azure.

<!--Download your publish settings from [here](https://manage.windowsazure.com/publishsettings/).-->
Stáhnout vaše nastavení pomocí PowerShell Azure hello publikování [Get-AzurePublishSettingsFile](https://docs.microsoft.com/en-us/powershell/module/azure/get-azurepublishsettingsfile?view=azuresmps-4.0.0) příkaz. 

Uložte hello soubor nastavení publikování v C:\chef.

## <a name="creating-a-managed-chef-account"></a>Vytvoření spravované Chef účtu
Zaregistrujte si účet hostované Chef [zde](https://manage.chef.io/signup).

Během procesu registrace hello, nebudete vyzváni toocreate nové organizace.

![][3]

Po vytvoření vaší organizace stáhněte hello starter kit.

![][4]

> [!NOTE]
> Pokud se zobrazí výzva, upozornění, že vaše klíče bude restartován, je ok tooproceed jako máme nakonfigurovanou zatím žádná existující infrastrukturu.
> 
> 

Tento soubor zip starter kit obsahuje vaše organizace konfigurační soubory a klíče.

## <a name="configuring-hello-chef-workstation"></a>Konfigurace hello Chef pracovní stanice
Extrahujte obsah hello tooC:\chef chef starter.zip hello.

Zkopírujte všechny soubory pod chef. starter\chef úložišti\.chef tooyour c:\chef adresáře.

Adresáře by měl nyní vypadat podobně jako následující příklad hello.

![][5]

Teď byste měli mít čtyři soubory včetně hello Azure publikování hello kořenové c:\chef.

soubory PEM Hello obsahovat vaší organizace a správy privátních klíčů pro komunikaci, zatímco hello knife.rb soubor obsahuje konfiguraci nůž. Potřebujeme tooedit hello knife.rb souboru.

Otevřete soubor hello ve svém editoru volba a upravit hello "cookbook_path" odebráním hello nebo... z hello cestu, zdá se, jak ukazuje následující.

    cookbook_path  ["#{current_dir}/cookbooks"]

Také přidat hello následující řádek odrážející hello název vaší Azure soubor nastavení publikování.

    knife[:azure_publish_settings_file] = "yourfilename.publishsettings"

Váš soubor knife.rb by teď měl vypadat podobně jako toohello následující ukázka.

![][6]

Tyto řádky zajišťují, že odkazuje na adresář cookbooks hello c:\chef\cookbooks nůž a také naše soubor s nastavením publikování Azure používá během operace v Azure.

## <a name="installing-hello-chef-development-kit"></a>Instalace hello Chef Development Kit
Další [stáhněte a nainstalujte](http://downloads.getchef.com/chef-dk/windows) hello tooset ChefDK (Chef Development Kit) do pracovní stanice Chef.

![][7]

Ve výchozím umístěním hello c:\opscode nainstalujte. Tato instalace bude trvat přibližně 10 minut.

Potvrďte, že vaše proměnná PATH obsahuje položky pro C:\opscode\chefdk\bin; C:\opscode\chefdk\embedded\bin;c:\users\yourusername\.chefdk\gem\ruby\2.0.0\bin

Pokud nejsou existuje, ujistěte se, že přidáte tyto cesty!

*Všimněte si hello pořadí z hello cesta je důležité!* Pokud vaše opscode cesty nejsou ve správném pořadí hello budete mít problémy.

Než budete pokračovat, restartujte počítač pracovní stanice.

V dalším kroku se nainstaluje rozšíření Azure nůž hello. To poskytuje nůž hello "Modul plug-in Azure".

Spusťte následující příkaz hello.

    chef gem install knife-azure ––pre

> [!NOTE]
> argument – před Hello zajistí, že se vám hello nejnovější verzi RC sad hello nůž modul plug-in Azure, který poskytuje přístup toohello nejnovější sadu rozhraní API.
> 
> 

Je pravděpodobné, počet závislosti budou také instalována na hello stejnou dobu.

![][8]

tooensure všechno, co je nakonfigurována správně, spusťte hello následující příkaz.

    knife azure image list

Pokud se vše správně nakonfigurovaná, zobrazí se seznam dostupných imagí Azure procházení.

Blahopřejeme. pracovní stanice Hello nastavení!

## <a name="creating-a-cookbook"></a>Vytváření kuchařka
Používá kuchařka Chef toodefine sadu příkazů na vašeho klienta spravovaného chcete tooexecute. Vytváření kuchařka je jednoduchý a používáme hello **chef generovat kuchařka** příkaz toogenerate naše kuchařka šablony. I bude možné volání Moje kuchařka webový server jako nastavit jako zásadu, která automaticky nasadí služby IIS.

V části adresáře C:\Chef spusťte následující příkaz hello.

    chef generate cookbook webserver

Tím se vygeneruje určitou sadu souborů v adresáři hello C:\Chef\cookbooks\webserver. Nyní potřebujeme toodefine hello sadu příkazů, že naše tooexecute klienta Chef rádi bychom znali na našem spravované virtuálního počítače.

Hello příkazy jsou uloženy v souboru default.rb hello. V tomto souboru I budete definování sadu příkazů, který nainstaluje službu IIS, spustí službu IIS a zkopíruje složku wwwroot toohello souboru šablony.

Upravte soubor C:\chef\cookbooks\webserver\recipes\default.rb hello a přidejte následující řádky hello.

    powershell_script 'Install IIS' do
         action :run
         code 'add-windowsfeature Web-Server'
    end

    service 'w3svc' do
         action [ :enable, :start ]
    end

    template 'c:\inetpub\wwwroot\Default.htm' do
         source 'Default.htm.erb'
         rights :read, 'Everyone'
    end

Až budete hotoví, uložte soubor hello.

## <a name="creating-a-template"></a>Vytvoření šablony
Jak jsme už bylo zmíněno dříve, potřebujeme toogenerate soubor šablony, který se použije jako naší stránce s default.html.

Spusťte následující příkaz toogenerate hello šablony hello.

    chef generate template webserver Default.htm

Nyní přejděte toohello C:\chef\cookbooks\webserver\templates\default\Default.htm.erb souboru. Upravte soubor hello přidáním jednoduchý "Hello, World" HTML kód a potom uložte soubor hello.

## <a name="upload-hello-cookbook-toohello-chef-server"></a>Nahrát hello kuchařka toohello Chef serveru
V tomto kroku jsme jsou pořízení kopie hello Cookbook, který jsme vytvořili na našem místního počítače a pak ho nahrát toohello Chef Hosted serveru. Po nahrání hello kuchařka se zobrazí v části hello **zásad** kartě.

    knife cookbook upload webserver

![][9]

## <a name="deploy-a-virtual-machine-with-knife-azure"></a>Nasazení virtuálního počítače s nůž Azure
Nyní změníme nasadit virtuální počítač Azure a použít hello kuchařka "Webový server", který nainstaluje naše IIS webové služby a výchozí webové stránky.

V pořadí toodo to, použijte hello **server nůž azure vytvořit** příkaz.

Se, že příklad hello příkazu se zobrazí další.

    knife azure server create --azure-dns-name 'diegotest01' --azure-vm-name 'testserver01' --azure-vm-size 'Small' --azure-storage-account 'portalvhdsxxxx' --bootstrap-protocol 'cloud-api' --azure-source-image 'a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-Datacenter-201411.01-en.us-127GB.vhd' --azure-service-location 'Southeast Asia' --winrm-user azureuser --winrm-password 'myPassword123' --tcp-endpoints 80,3389 --r 'recipe[webserver]'

Parametry Hello je zřejmých. Nahraďte konkrétní proměnných a spustit.

> [!NOTE]
> Prostřednictvím hello hello příkazového řádku I se také automatizaci Moje pravidla filtru koncového bodu sítě pomocí parametru koncové body – tcp hello. Jste otevřené porty 80 a 3389 tooprovide přístup toomy webové stránky a relaci protokolu RDP.
> 
> 

Po spuštění příkazu hello přejděte toohello Azure portal a můžete se zobrazí váš počítač začít tooprovision.

![][13]

Hello příkazového řádku se zobrazí na další.

![][10]

Po dokončení nasazení hello jsme musí být schopný tooconnect toohello webové služby přes port 80 při jsme zřízení hello virtuální počítač s hello nůž Azure příkaz jsme měl otevřít hello port. Tento virtuální počítač je hello pouze virtuální počítač v mé cloudové služby, budete ho připojení s adresou url služby cloud hello.

![][11]

Jak vidíte, se zobrazí chybové tvůrčí s vlastního kódu HTML.

Nezapomeňte, že jsme také můžete připojit přes relaci protokolu RDP z portálu Azure přes port 3389 hello.

Ať, že tento byl užitečné! Přejděte a spusťte infrastruktury jako cesty kódu s Azure ještě dnes!

<!--Image references-->
[2]: media/chef-automation/2.png
[3]: media/chef-automation/3.png
[4]: media/chef-automation/4.png
[5]: media/chef-automation/5.png
[6]: media/chef-automation/6.png
[7]: media/chef-automation/7.png
[8]: media/chef-automation/8.png
[9]: media/chef-automation/9.png
[10]: media/chef-automation/10.png
[11]: media/chef-automation/11.png
[13]: media/chef-automation/13.png


<!--Link references-->
