---
title: "aaaDeploy tooLinux aplikace Node.js virtuálních počítačů v Azure"
description: "Zjistěte, jak toodeploy Node.js aplikace tooLinux virtuálních počítačů v Azure."
services: 
documentationcenter: nodejs
author: stepro
manager: dmitryr
editor: 
ROBOTS: NOINDEX, NOFOLLOW
redirect_url: /azure
ms.assetid: 857a812d-c73e-4af7-a985-2d0baf8b6f71
ms.service: multiple
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/02/2016
ms.author: stephpr
ms.openlocfilehash: e876c8170a06f102f7f32ec7cb930b876647a707
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-nodejs-application-toolinux-virtual-machines-in-azure"></a>Nasazení tooLinux aplikace Node.js virtuálních počítačů v Azure
Tento kurz ukazuje, jak tootake aplikace Node.js a nasadit ji tooLinux virtuální počítače běžící v Azure. Hello pokyny v tomto kurzu platí pro všechny operační systémy, který je schopen spuštění Node.js.

Dozvíte, jak:

* Rozvětvení a klonování úložiště GitHub obsahující jednoduchou aplikaci TODO;
* Vytvořit a nakonfigurovat dvě virtuální počítače s Linuxem v Azure toorun hello aplikace;
* Iterate hello aplikace nuceným doručením aktualizace toohello webového front-endu virtuálního počítače.

> [!NOTE]
> toocomplete tohoto kurzu potřebujete účet GitHub a účet Microsoft Azure a hello možnost toouse Git z vývojovém počítači.
> 
> Pokud nemáte účet Githubu, můžete si zaregistrovat [zde](https://github.com/join).
> 
> Pokud nemáte [Microsoft Azure](https://azure.microsoft.com/) účet, můžete si zaregistrovat bezplatnou zkušební verzi [zde](https://azure.microsoft.com/pricing/free-trial/). To také vás provede hello registrační proces [Account Microsoft](http://account.microsoft.com) Pokud jste již nemají jeden. Případně, pokud jste předplatitele sady Visual Studio, můžete [aktivovat výhody webu MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).
> 
> Pokud nemáte git na vývojovém počítači, pokud používáte počítač Macintosh nebo Windows, nainstalujte git z [zde](http://www.git-scm.com). Pokud používáte Linux, nainstalovat git pomocí hello mechanismus nejlépe vyhovuje, například `sudo apt-get install git`.
> 
> 

## <a name="forking-and-cloning-hello-todo-application"></a>Větvení a klonování hello aplikace TODO
Hello aplikace TODO používané v tomto kurzu implementuje jednoduchý webový front-end přes instance MongoDB, které uchovává informace o seznamu úkolů. Po přihlášení tooGitHub, přejděte [sem](https://github.com/stepro/node-todo) toofind hello aplikace a rozvětvit pomocí hello odkaz v pravé horní hello. To by měl vytvořit úložiště ve vašem účtu s názvem *accountname*/node-todo.

Nyní na vývojovém počítači, klonování toto úložiště:

    git clone https://github.com/accountname/node-todo.git

Použijeme Tenhle klon. místní úložiště hello trochu později při provádění změn toohello zdrojového kódu.

## <a name="creating-and-configuring-hello-linux-virtual-machines"></a>Vytváření a konfiguraci hello virtuální počítače s Linuxem
Azure má podpory pro nezpracovanou výpočet pomocí virtuální počítače s Linuxem. Tato část hello kurz ukazuje, jak můžete snadno číselníku až dva virtuální počítače s Linuxem a nasadit toothem aplikace hello TODO, spuštěné hello webového front-endu na jednu a hello instance MongoDB na hello jiné.

### <a name="creating-virtual-machines"></a>Vytváření virtuálních počítačů
Nejjednodušší způsob, jak toocreate Hello nového virtuálního počítače v Azure je toouse hello portálu Azure. Klikněte na tlačítko [sem](https://portal.azure.com) toosign v a spouštět hello portálu Azure ve vašem webovém prohlížeči. Jakmile se načetl hello portálu Azure, dokončete hello následující kroky:

* Klikněte na tlačítko hello "+ nové" odkaz;
* Vyberte kategorie "Výpočetní" hello a zvolte "Ubuntu Server 14.04 LTS";
* Vyberte model nasazení hello "Resource Manager" a klikněte na tlačítko "Vytvořit";
* Vyplňte hello základy následujících pokynů:
  * Zadejte název, který vám později mohli snadno identifikovat;
  * V tomto kurzu zvolte ověřování hesla;
  * Vytvořte novou skupinu prostředků s názvem Osobní.
* Pro hello velikost virtuálního počítače "A1 standardní" je vhodná volba pro účely tohoto kurzu.
* Další nastavení Ujistěte se, že typ disku hello "Standard" a přijmout že všechny hello zbývající výchozí hodnoty.
* Ji hello vytvoření na souhrnné stránce hello.

Provedení hello výše zpracovat dvakrát toocreate dva Linuxové virtuální počítače, jeden pro front-end webové hello a jeden pro hello MongoDB instance. Vytváření virtuálních počítačů hello bude trvat přibližně 5 až 10 minut.

### <a name="assigning-a-dns-entry-for-virtual-machines"></a>Přiřazení položky DNS pro virtuální počítače
Virtuální počítače vytvořené v Azure jsou ve výchozím nastavení je dostupné jenom přes veřejnou IP adresu jako 1.2.3.4. Přiřazením záznamy DNS vytvoříme snadněji osobní počítače hello.

Jakmile hello portál označuje, zda byly vytvořeny hello virtuální počítače, klikněte na odkaz "Virtuální počítače" hello v levé navigační panel hello a vyhledejte vašich počítačů. Pro každý počítač:

* Vyhledejte hello Essentials kartě a klikněte na hello veřejnou IP adresu.
* V hello veřejné konfiguraci IP adresy přiřaďte Popisek názvu DNS a uložit.

Hello portálu se ujistěte se, že je k dispozici tento hello název, který zadáte. Po uložení konfigurace hello, virtuální počítače bude mít názvy hostitelů, které jsou podobné příliš`machinename.region.cloudapp.azure.com`.

### <a name="connecting-toohello-virtual-machines"></a>Připojení toohello virtuální počítače
Virtuální počítače byly zajištěny, byly předem nakonfigurovaná tooallow vzdálená připojení přes protokol SSH. Toto je mechanismus hello použijeme tooconfigure hello virtuálních počítačů. Pokud používáte systém Windows pro váš vývojový, tooget klientem SSH je nutné, pokud jste již nemají jeden. Běžné volba je PuTTY, který si můžete stáhnout z [zde](http://www.chiark.greenend.org.uk/~sgtatham/putty/). Macintosh a operační systémy Linux obsahují verzi předinstalovaným SSH.

### <a name="configuring-hello-web-frontend-virtual-machine"></a>Konfigurace hello virtuálním počítačem webového front-endu
SSH toohello webové front-endu počítače, který jste vytvořili pomocí klienta PuTTY, ssh příkazového řádku nebo vaše jiný nástroj Oblíbené SSH. Měli byste vidět uvítací zprávy, za nímž následuje příkazový řádek.

První Ujistěte se, zda git a uzlu jsou oba nainstalovány:

    sudo apt-get install -y git
    curl -sL https://deb.nodesource.com/setup_4.x | sudo -E bash -
    sudo apt-get install -y nodejs

Vzhledem k tomu, že front-endu webové aplikace hello závisí na některé nativních modulů Node.js, potřebujeme také tooinstall hello základní sadu nástrojů sestavení:

    sudo apt-get install -y build-essential

Nakonec nainstalujme aplikaci Node.js s názvem *navždy*, což pomáhá toorun Node.js serverové aplikace:

    sudo npm install -g forever

Toto jsou všechny závislosti hello na tento virtuální počítač toobe možné toorun hello aplikace webového front-endu, takže Pojďme se systémem. toodo, nejdřív vytvoříme úplné klon úložiště GitHub hello dříve forked, aby mohli snadno publikovat aktualizace toohello virtuálního počítače (jsme zaměříme tento scénář aktualizace později) a poté klonovat hello úplné klon tooprovide verzi hello úložiště, ve skutečnosti může být spuštěn.

Spouštění z adresáře hello home (~), spusťte následující příkazy hello (nahrazení *accountname* pomocí názvu uživatelského účtu Githubu):

    git clone --bare https://github.com/accountname/node-todo.git
    git clone node-todo.git

Nyní zadejte adresář hello uzlu úkolů a spusťte tyto příkazy:

    npm install
    forever start server.js

Hello front-endu webové aplikace je nyní spuštěna, ale je jeden krok před přístupem k aplikaci hello z webového prohlížeče. Hello vytvoříte virtuální počítač je chráněn prostředek služby Azure, názvem *skupinu zabezpečení sítě*, který byl vytvořen pro případě jste zřídili hello virtuálního počítače. Tento prostředek v současné době umožňuje pouze externí požadavky tooport 22 toobe směrovány toohello virtuálních počítačů umožňující komunikaci SSH s hello počítač, ale nic jiného. V hello tooview pořadí úkolů aplikace, která je nakonfigurovaná toorun na portu 8080, je proto tento port taky musí toobe otevřeli.

Vraťte se toohello portálu Azure a dokončete hello následující kroky:

* Klikněte na "Prostředku skupiny" v levé navigační panel hello;
* Vyberte skupinu prostředků hello, která obsahuje váš virtuální počítač;
* V hello výsledného seznamu prostředků vyberte skupinu zabezpečení sítě hello (hello jeden ikona štítu);
* V dialogovém okně Vlastnosti hello zvolte "pravidla zabezpečení příchozích";
* V panelu nástrojů hello klikněte na tlačítko "Přidat";
* Zadejte název jako "výchozí povolit todo";
* Nastavení protokolu hello příliš "TCP";
* Nastavit rozsah cílových portů hello příliš "8080";
* Klikněte na tlačítko OK a počkejte toobe pravidlo zabezpečení hello vytvořili.

Po vytvoření tohoto pravidla zabezpečení, je veřejně viditelný na hello aplikace TODO hello internet a můžete procházet tooit, například pomocí adresy URL, například:

    http://machinename.region.cloudapp.azure.com:8080

Si všimnete, že i když ještě nenakonfigurovali hello MongoDB virtuálního počítače, zobrazí hello aplikace TODO toobe poměrně funkční. Je to proto hello zdrojové úložiště je pevně zakódované toouse předem nasazené instance MongoDB. Jakmile jsme nakonfigurovali hello MongoDB virtuálního počítače, jsme se přejděte zpět a změňte hello zdrojového kódu tooutilize naše privátní instance MongoDB místo.

### <a name="configuring-hello-mongodb-virtual-machine"></a>Konfigurace hello MongoDB virtuálního počítače
SSH toohello druhý počítač, který jste vytvořili pomocí klienta PuTTY, ssh příkazového řádku nebo vaše jiný nástroj Oblíbené SSH. Po zobrazení uvítací zprávy hello a příkazového řádku, nainstalujte MongoDB (tyto pokyny, které byly odebrány z [sem](https://docs.mongodb.org/master/tutorial/install-mongodb-on-ubuntu/)):

    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv EA312927
    echo "deb http://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.2.list
    sudo apt-get update
    sudo apt-get install -y mongodb-org

Ve výchozím nastavení je nakonfigurovaný MongoDB, takže ho lze přistupovat pouze místně. V tomto kurzu jsme se nakonfigurujte MongoDB tak, aby byla přístupná z aplikace hello virtuálního počítače. V kontextu sudo, otevřete soubor /etc/mongod.conf hello a vyhledejte hello `# network interfaces` části. Změna hello `net.bindIp` konfigurace hodnota příliš`0.0.0.0`.

> [!NOTE]
> Tato konfigurace je pro účely tohoto kurzu pouze hello. Je **není** doporučené postupy zabezpečení a neměl by být používat v produkčním prostředí.
> 
> 

Teď zajistěte hello MongoDB byla spuštěna služba:

    sudo service mongod restart

MongoDB funguje přes port 27017 ve výchozím nastavení. Proto v hello stejným způsobem, že jsme potřeby tooopen port 8080 na virtuální počítač hello webového front-endu, potřebujeme tooopen port 27017 hello MongoDB virtuálního počítače.

Vraťte se toohello portálu Azure a dokončete hello následující kroky:

* Klikněte na "Prostředku skupiny" v levé navigační panel hello;
* Vyberte skupinu prostředků hello, který obsahuje hello MongoDB virtuální počítač;
* V hello výsledného seznamu prostředků vyberte skupinu zabezpečení sítě hello (hello jeden ikona štítu) s hello stejný název, který jste zadali v toohello MongoDB virtuálního počítače;
* V dialogovém okně Vlastnosti hello zvolte "pravidla zabezpečení příchozích";
* V panelu nástrojů hello klikněte na tlačítko "Přidat";
* Zadejte název jako "výchozí povolit mongo";
* Nastavení protokolu hello příliš "TCP";
* Nastavit rozsah cílových portů hello příliš "27017";
* Klikněte na tlačítko OK a počkejte toobe pravidlo zabezpečení hello vytvořili.

## <a name="iterating-on-hello-todo-application"></a>Iterace v hello aplikace TODO
Zatím jsme máte zřízen dva virtuální počítače Linux: ten, který je spuštěna aplikace hello front-endové webové a jeden, je spuštěna MongoDB instance. Ale došlo k potížím – front-endu webové hello nepoužívá ve skutečnosti instance MongoDB hello zřízený ještě. Umožňuje vyřešit, aktualizací toouse kód front-endu webové hello proměnné prostředí místo je pevně instance.

### <a name="changing-hello-todo-application"></a>Změna aplikace TODO hello
Na vývojovém počítači kde nejprve klonovat úložiště hello uzlu todo, otevřete hello `node-todo/config/database.js` souboru ve svém oblíbeném editoru a změňte hodnotu adresy url hello z hello pevně hodnotu jako `mongodb://...` příliš`process.env.MONGODB`.

Potvrďte změny a odešlete hlavní toohello Githubu:

    git commit -am "Get MongoDB instance from env"
    git push origin master

Bohužel tuto nepodporuje publikování hello změnu toohello webového front-endu virtuálního počítače. Provedeme pár další tooenable virtuální počítač toothat změny jednoduchý, ale efektivní mechanismus pro publikování aktualizací, takže můžete rychle sledovat účinek hello hello změny v prostředí za provozu hello.

### <a name="configuring-hello-web-frontend-virtual-machine"></a>Konfigurace hello virtuálním počítačem webového front-endu
Odvolat jsme dříve vytvořili úplné klon hello todo uzlu úložiště na virtuální počítač hello webového front-endu. Ukazuje se, že tato akce vytvořeny nové Git vzdálené toowhich změny mohou poslat. Ale jednoduše vkládání toothis vzdálené poměrně nedává hello rychlé iterace model, který vývojářům hledáte při práci na svůj kód.

Co jsme chcete mít toodo toobe je zajistěte, aby při nabízené toohello vzdáleného úložiště na virtuálním počítači hello, běžící aplikaci TODO hello automaticky aktualizovala. Naštěstí jde snadno tooachieve s gitem.

Git udává počet háky, jež jsou volány v určitou dobu tooreact tooactions pořízené hello úložiště. Tyto jsou určeny pomocí skriptů prostředí v úložišti hello `hooks` složky. Hello háku, který je pro scénář automatickou aktualizaci hello tehdy, je hello `post-update` událostí.

SSH relace toohello webového front-endu virtuální počítač, změňte toohello `~/node-todo.git/hooks` adresáře a přidejte následující obsahu tooa soubor s názvem hello `post-update` (nahrazení `machinename` a `region` s vaše informace o virtuálním počítači MongoDB):

    #!/bin/bash

    forever stopall
    unset 'GIT_DIR'
    export MONGODB="mongodb://machinename.region.cloudapp.azure.com:27017/tododb"
    cd ~/node-todo && git fetch origin && git pull origin master && npm install && forever start ~/node-todo/server.js
    exec git update-server-info

Zkontrolujte, zda že tento soubor je spustitelný soubor spuštěním hello následující příkaz:

    chmod 755 post-update

Tento skript zajišťuje, že aktuální aplikace server hello je zastavena, hello kód v úložišti klonovaný hello je aktualizovaný toohello nejnovější, žádné aktualizované závislostí a restartování serveru hello. Kromě toho zajišťuje, že byl nakonfigurován hello prostředí v rámci přípravy pro příjem naše první aktualizace tooget hello MongoDB instanci aplikace na proměnné prostředí.

### <a name="configuring-your-development-machine"></a>Konfigurace počítači pro vývoj
Nyní Pojďme vývojovém počítači připojili toohello webového front-endu virtuálního počítače. To je jednoduché, přidání hello úplné úložiště na virtuálním počítači hello jako vzdálené. Spusťte následující příkaz toodo hello to (nahrazení *uživatele* s vaše přihlašovací jméno webového front-endu virtuálního počítače a *machinename* a *oblast* podle potřeby):

    git remote add azure user@machinename.region.cloudapp.azure.com:node-todo.git

To je vše, který je potřebný tooenable vkládání, nebo v platnosti publikování, změny virtuálního počítače toohello webového front-endu.

### <a name="publishing-updates"></a>Publikování aktualizací
Umožňuje publikovat hello jeden změn, který byl změněn, pokud tak, aby hello aplikace bude používat vlastní instance MongoDB:

    git push azure master

Měli byste vidět výstup podobný toothis:

    Counting objects: 4, done.
    Delta compression using up too4 threads.
    Compressing objects: 100% (3/3), done.
    Writing objects: 100% (4/4), 406 bytes | 0 bytes/s, done.
    Total 4 (delta 1), reused 0 (delta 0)
    remote: info:    Forever stopped processes:
    remote: data:        uid  command         script    forever pid  id logfile                         uptime
    remote: data:    [0] 0Lyh /usr/bin/nodejs server.js 9064    9301    /home/username/.forever/0Lyh.log 0:0:3:17.487
    remote: From /home/username/node-todo
    remote:    5f31fd7..5bc7be5  master     -> origin/master
    remote: From /home/username/node-todo
    remote:  * branch            master     -> FETCH_HEAD
    remote: Updating 5f31fd7..5bc7be5
    remote: Fast-forward
    remote:  config/database.js | 2 +-
    remote:  1 file changed, 1 insertion(+), 1 deletion(-)
    remote: npm WARN package.json node-todo@0.0.0 No repository field.
    remote: npm WARN package.json node-todo@0.0.0 No license field.
    remote: warn:    --minUptime not set. Defaulting to: 1000ms
    remote: warn:    --spinSleepTime not set. Your script will exit if it does not stay up for at least 1000ms
    remote: info:    Forever processing file: /home/username/node-todo/server.js
    toousername@machinename.region.cloudapp.azure.com:node-todo.git
    5f31fd7..5bc7be5  master -> master

Po dokončení tohoto příkazu, aktualizujte aplikace hello ve webovém prohlížeči. Musí být schopný toosee, že seznam úkolů hello okomentovat je prázdný a už nasazené vázanou toohello sdílené MongoDB instance.

toocomplete hello kurzu provedeme jiný, více viditelných změnu. Na vývojovém počítači otevřete soubor node-todo/public/index.html hello pomocí vašeho oblíbeného editoru. Vyhledejte hello jumbotron záhlaví a změňte název hello z "Jsem Todo-aholic" příliš "jsem aholic Todo v Azure!".

Teď umožňuje zapsat:

    git commit -am "Azurify hello title"

Tento čas, budeme publikovat hello změnu tooAzure před odesláním ji úložiště GitHub toohello tooback:

    git push azure master

Po dokončení tohoto příkazu se zobrazí webovou stránku hello aktualizace a změny hello. Vzhledem k tomu, že budou vypadat dobře, push vzdálené hello změnu zpět toohello původu: 

    git push origin master

## <a name="next-steps"></a>Další kroky
Tento článek vám ukázal, jak tootake aplikace Node.js a nasadit ji tooLinux virtuální počítače běžící v Azure. toolearn Další informace o virtuálních počítačích s Linuxem v Azure, najdete v části [tooLinux Úvod v Azure](/documentation/articles/virtual-machines-linux-introduction/).

Další informace o tom, toodevelop aplikací Node.js v Azure, najdete v části hello [středisku pro vývojáře Node.js](/develop/nodejs/).

