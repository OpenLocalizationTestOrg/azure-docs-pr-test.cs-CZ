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
# <a name="ruby-on-rails-web-application-on-an-azure-vm"></a>Webová aplikace Ruby on Rails na virtuálním počítači Azure
Tento kurz ukazuje, jak toohost Ruby, na které webu v Azure pomocí virtuální počítač s Linuxem.  

V tomto kurzu se ověřila pomocí Ubuntu Server 14.04 LTS. Pokud používáte jiný distribuční Linux, bude pravděpodobně nutné toomodify hello kroky, které tooinstall.

> [!IMPORTANT]
> Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../../../azure-resource-manager/resource-manager-deployment-model.md).  Tento článek se zabývá pomocí modelu nasazení classic hello. Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.
>
>

## <a name="create-an-azure-vm"></a>Vytvoření virtuálního počítače Azure
Začněte vytvořením virtuálního počítače Azure s bitovou kopii systému Linux.

toocreate hello virtuálních počítačů, můžete použít hello portál Azure nebo hello rozhraní příkazového řádku Azure (CLI).

### <a name="azure-portal"></a>portál Azure
1. Přihlaste se k hello [portálu Azure](https://portal.azure.com)
2. Klikněte na tlačítko **nový**, hello vyhledávacího pole zadejte "Ubuntu Server 14.04". Klikněte na položku hello vrácený hello vyhledávání. Pro model nasazení hello, vyberte **Classic**, pak klikněte na tlačítko "Vytvořit".
3. V okně základy hello, zadejte hodnoty pro pole hello požadované: název (pro hello virtuálních počítačů), uživatelské jméno, typ ověřování a přihlašovací údaje odpovídající hello, předplatné, skupinu prostředků a umístění.

   ![Vytvořit novou bitovou kopii Ubuntu](./media/virtual-machines-linux-classic-ruby-rails-web-app/createvm.png)

4. Po zřízení hello virtuálních počítačů, klikněte na název virtuálního počítače hello a klikněte na tlačítko **koncové body** v hello **nastavení** kategorie. Najít hello SSH koncový bod, uvedené v části **samostatné**.

   ![Výchozí koncový bod](./media/virtual-machines-linux-classic-ruby-rails-web-app/endpointsnewportal.png)

### <a name="azure-cli"></a>Azure CLI
Postupujte podle kroků hello v [vytvořit virtuálním počítači systémem Linux][vm-instructions].

Po zřízení hello virtuálních počítačů můžete získat koncový bod SSH hello spuštěním hello následující příkaz:

    azure vm endpoint list <vm-name>  

## <a name="install-ruby-on-rails"></a>Nainstalujte na které Ruby
1. Pomocí SSH tooconnect toohello virtuálních počítačů.
2. Z relace SSH hello použijte následující příkazy tooinstall Ruby na hello virtuálních počítačů hello:

        sudo apt-get update -y
        sudo apt-get upgrade -y

        sudo apt-add-repository ppa:brightbox/ruby-ng
        sudo apt-get update
        sudo apt-get install ruby2.4

        > [!TIP]
        > hello brightbox repository contains hello current Ruby distribution.

    Hello instalace může trvat několik minut. Po dokončení, použijte následující příkaz tooverify nainstalovanou Ruby hello:

        ruby -v

3. Použití hello následující příkaz tooinstall které:

        sudo gem install rails --no-rdoc --no-ri -V

    Použijte hello – ne rdoc a--ne ri příznaky tooskip instalaci hello dokumentace, která je rychlejší.
    Tento příkaz bude pravděpodobně trvat dlouhou dobu tooexecute, takže přidání hello -V se zobrazí informace o průběhu instalace hello.

## <a name="create-and-run-an-app"></a>Vytvoření a spuštění aplikace
Při přihlášení se stále pomocí protokolu SSH, spusťte následující příkazy hello:

    rails new myapp
    cd myapp
    rails server -b 0.0.0.0 -p 3000

Hello [nové](http://guides.rubyonrails.org/command_line.html#rails-new) příkaz vytvoří novou aplikaci které. Hello [server](http://guides.rubyonrails.org/command_line.html#rails-server) příkaz spustí hello WEBrick webový server, která se dodává s které. (Pro použití v provozním prostředí, by pravděpodobně chcete toouse jiný server, jako je například Unicorn nebo osobní.)

Měli byste vidět výstup podobný toohello následující.

    => Booting WEBrick
    => Rails 4.2.1 application starting in development on http://0.0.0.0:3000
    => Run `rails server -h` for more startup options
    => Ctrl-C tooshutdown server
    [2015-06-09 23:34:23] INFO  WEBrick 1.3.1
    [2015-06-09 23:34:23] INFO  ruby 1.9.3 (2013-11-22) [x86_64-linux]
    [2015-06-09 23:34:23] INFO  WEBrick::HTTPServer#start: pid=27766 port=3000

## <a name="add-an-endpoint"></a>Přidání koncového bodu
1. Přejděte toohello [portál Azure] [https://portal.azure.com] a vyberte virtuální počítač.

2. Vyberte **koncové body** v hello **nastavení** podél stránku hello hello levý okraj.

3. Klikněte na tlačítko **přidat** hello horní části stránky hello.

4. V hello **přidání koncového bodu** dialogovém okně zadejte hello následující informace:

   * **Název**: HTTP
   * **Protokol**: TCP
   * **Veřejný port**: 80
   * **Privátní port**: 3000
   * **Plovoucí PÍ adresu**: zakázáno
   * **Seznam řízení přístupu – pořadí**: 1001 nebo jinou hodnotu, která nastavuje prioritu hello toto přístupové pravidlo.
   * **Seznam řízení přístupu – název**: allowHTTP
   * **Seznam řízení přístupu – akce**: Povolit
   * **Seznam řízení přístupu – vzdálené podsíti**: 1.0.0.0/16

     Tento koncový bod má veřejný port 80, který bude směrovat provoz toohello privátní port 3000, kde hello které server naslouchá. Hello pravidlo seznamu řízení přístupu umožňuje veřejné přenosy na portu 80.

     ![Nový koncový bod](./media/virtual-machines-linux-classic-ruby-rails-web-app/createendpoint.png)

5. Klikněte na tlačítko OK toosave hello koncový bod.

6. Měli zobrazí zpráva s oznámením **ukládání koncový bod virtuálního počítače**. Jakmile se tato zpráva zmizí, koncový bod hello je aktivní. Teď může otestovat aplikaci tak, že přejdete toohello název DNS virtuálního počítače. Hello webu by měla vypadat podobně jako toohello následující:

    ![Výchozí stránka které][default-rails-cloud]

## <a name="next-steps"></a>Další kroky
V tomto kurzu jste to udělali většinu hello kroky ručně. V produkčním prostředí by zapsat aplikaci na vývojovém počítači a nasaďte ji toohello virtuálního počítače Azure. Navíc většině produkčních prostředích hostování aplikace, které hello ve spojení s jiným procesem serveru, jako je například Apache či NginX, která zpracovává žádosti směrování toomultiple instance hello které aplikace a obsluhuje statické prostředky. Další informace najdete v tématu http://rubyonrails.org/deploy/.

toolearn Další informace o Ruby, na které, navštivte hello [Ruby, na které příručky][rails-guides].

toouse Azure služeb z Ruby aplikace naleznete v tématu:

* [Ukládání nestrukturovaných dat pomocí objektů BLOB][blobs]
* [Páry klíč – hodnota úložiště pomocí tabulky][tables]
* [Poskytovat obsah velkou šířku pásma s hello sítě pro doručování obsahu][cdn-howto]

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
