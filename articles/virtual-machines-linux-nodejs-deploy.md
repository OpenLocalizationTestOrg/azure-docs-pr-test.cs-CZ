---
title: "Nasazení aplikace Node.js pro virtuální počítače s Linuxem v Azure"
description: "Informace o nasazení aplikace Node.js na virtuální počítače s Linuxem v Azure."
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
ms.openlocfilehash: d3d9fecfafb9ba422420230716b9c985481d1e24
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-a-nodejs-application-to-linux-virtual-machines-in-azure"></a>Nasazení aplikace Node.js pro virtuální počítače s Linuxem v Azure
Tento kurz ukazuje, jak provést aplikace Node.js a nasadíte ho do Linuxové virtuální počítače běžící v Azure. Pokyny v tomto kurzu platí pro všechny operační systémy, které podporují Node.js.

Dozvíte, jak:

* Rozvětvení a klonování úložiště GitHub obsahující jednoduchou aplikaci TODO;
* Vytvořit a nakonfigurovat dvě virtuální počítače s Linuxem v Azure a spusťte aplikaci;
* Iterate zobrazením aktualizací na virtuální počítač front-endu webové aplikace.

> [!NOTE]
> K dokončení tohoto kurzu, budete potřebovat účet GitHub a účet Microsoft Azure a možnost používat Git z vývojovém počítači.
> 
> Pokud nemáte účet Githubu, můžete si zaregistrovat [zde](https://github.com/join).
> 
> Pokud nemáte [Microsoft Azure](https://azure.microsoft.com/) účet, můžete si zaregistrovat bezplatnou zkušební verzi [zde](https://azure.microsoft.com/pricing/free-trial/). To také vás provede proces pro přihlášení [Account Microsoft](http://account.microsoft.com) Pokud jste již nemají jeden. Případně, pokud jste předplatitele sady Visual Studio, můžete [aktivovat výhody webu MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).
> 
> Pokud nemáte git na vývojovém počítači, pokud používáte počítač Macintosh nebo Windows, nainstalujte git z [zde](http://www.git-scm.com). Pokud používáte Linux, nainstalovat git pomocí mechanismu nejlépe vyhovuje, například `sudo apt-get install git`.
> 
> 

## <a name="forking-and-cloning-the-todo-application"></a>Větvení a klonování aplikace TODO
Aplikace TODO používané v tomto kurzu implementuje jednoduchý webový front-end přes instance MongoDB, které uchovává informace o seznamu úkolů. Po přihlášení na Githubu, přejděte [sem](https://github.com/stepro/node-todo) můžete najít aplikaci a rozvětvit ho pomocí odkazu v pravé horní. To by měl vytvořit úložiště ve vašem účtu s názvem *accountname*/node-todo.

Nyní na vývojovém počítači, klonování toto úložiště:

    git clone https://github.com/accountname/node-todo.git

Použijeme Tenhle klon. místní úložiště trochu později při provádění změn ke zdrojovému kódu.

## <a name="creating-and-configuring-the-linux-virtual-machines"></a>Vytváření a konfigurace virtuálního počítače se systémem Linux
Azure má podpory pro nezpracovanou výpočet pomocí virtuální počítače s Linuxem. Tato část kurz ukazuje, jak můžete snadno číselníku až dva virtuální počítače s Linuxem a nasadit aplikace TODO, front-endu webové systémem jednu a instance MongoDB na straně druhé.

### <a name="creating-virtual-machines"></a>Vytváření virtuálních počítačů
Nejjednodušší způsob, jak vytvořit nový virtuální počítač v Azure je pomocí webu Azure Portal. Klikněte na tlačítko [sem](https://portal.azure.com) přihlásit a spuštění portálu Azure ve vašem webovém prohlížeči. Jakmile načetl portálu Azure, proveďte následující kroky:

* Klikněte na odkaz "+ nové";
* Vyberte kategorie "Výpočet" a zvolte "Ubuntu Server 14.04 LTS";
* Vyberte model nasazení "Resource Manager" a klikněte na tlačítko "Vytvořit";
* Zadejte základní informace o následujících pokynů:
  * Zadejte název, který vám později mohli snadno identifikovat;
  * V tomto kurzu zvolte ověřování hesla;
  * Vytvořte novou skupinu prostředků s názvem Osobní.
* Pro velikost virtuálního počítače "A1 standardní" je vhodná volba pro účely tohoto kurzu.
* Další nastavení Ujistěte se, že typ disku "Standard" a přijměte všechny zbývající výchozí hodnoty.
* Ji vytvoření na stránce Souhrn.

Proveďte výše uvedený postup dvakrát vytvořit dva virtuální počítače Linux, jeden pro front-endu webové a jeden pro MongoDB instance. Vytváření virtuálních počítačů bude trvat přibližně 5 až 10 minut.

### <a name="assigning-a-dns-entry-for-virtual-machines"></a>Přiřazení položky DNS pro virtuální počítače
Virtuální počítače vytvořené v Azure jsou ve výchozím nastavení je dostupné jenom přes veřejnou IP adresu jako 1.2.3.4. Pojďme bylo na počítače snadněji identifikovat přiřazením záznamy DNS.

Jakmile na portál označuje, zda byly vytvořeny virtuální počítače, klikněte na odkaz "Virtuální počítače" v levé navigační panel a vyhledejte vašich počítačů. Pro každý počítač:

* Vyhledejte kartě základních a klikněte na veřejnou IP adresu;
* Ve veřejné konfiguraci IP adresy přiřaďte Popisek názvu DNS a uložit.

Na portálu zajistí, že je k dispozici název, který zadáte. Po uložení konfigurace, bude mít virtuální počítače názvy hostitelů, které jsou podobné `machinename.region.cloudapp.azure.com`.

### <a name="connecting-to-the-virtual-machines"></a>Připojení k virtuálním počítačům
Pokud vaše virtuální počítače byly zajištěny, byly předem nakonfigurovaný k povolení vzdálených připojení přes protokol SSH. Toto je mechanismus, který budeme používat ke konfiguraci virtuálních počítačů. Pokud používáte systém Windows pro váš vývojový, musíte získat klientem SSH, pokud jste již nemají jeden. Běžné volba je PuTTY, který si můžete stáhnout z [zde](http://www.chiark.greenend.org.uk/~sgtatham/putty/). Macintosh a operační systémy Linux obsahují verzi předinstalovaným SSH.

### <a name="configuring-the-web-frontend-virtual-machine"></a>Konfigurace virtuálního počítače webového front-endu
SSH k počítači front-endu webové, kterou jste vytvořili pomocí PuTTY, ssh příkazového řádku nebo vaše jiný nástroj Oblíbené SSH. Měli byste vidět uvítací zprávy, za nímž následuje příkazový řádek.

První Ujistěte se, zda git a uzlu jsou oba nainstalovány:

    sudo apt-get install -y git
    curl -sL https://deb.nodesource.com/setup_4.x | sudo -E bash -
    sudo apt-get install -y nodejs

Vzhledem k tomu, že front-endu webové aplikace závisí na některé nativních modulů Node.js, jsme také muset nainstalovat základní sadu nástrojů sestavení:

    sudo apt-get install -y build-essential

Nakonec nainstalujme aplikaci Node.js s názvem *navždy*, což přispívá ke spouštění aplikací Node.js serveru:

    sudo npm install -g forever

Toto jsou všechny závislosti nutné na tomto virtuálním počítači, abyste mohli spustit front-endu webové aplikace, takže Pojďme se systémem. Uděláte to tak, nejdřív vytvoříme úplné klon úložiště GitHub dříve forked, aby mohli snadno publikovat aktualizace virtuálního počítače (jsme zaměříme tento scénář aktualizace později) a poté klonovat úplné klon zadejte verzi úložiště, ve skutečnosti mohou být provedeny.

Spouštění z adresáře home (~), spusťte následující příkazy (nahrazení *accountname* pomocí názvu uživatelského účtu Githubu):

    git clone --bare https://github.com/accountname/node-todo.git
    git clone node-todo.git

Nyní zadejte adresář, do uzlu úkolů a spusťte tyto příkazy:

    npm install
    forever start server.js

Front-endu webové aplikace je nyní spuštěna, ale je jeden krok před přístupem k aplikaci z webového prohlížeče. Virtuální počítač, který jste vytvořili je chráněn prostředek služby Azure, názvem *skupinu zabezpečení sítě*, který byl vytvořen pro vás při zřizování virtuálního počítače. Tento prostředek v současné době umožňuje pouze externí požadavky na port 22 k přesměrování na virtuální počítač, který umožňuje komunikaci SSH s počítači, ale nic jiného. Chcete-li zobrazit aplikaci úkolů, která je nakonfigurovaná pro spuštění na portu 8080, tento port taky musí být otevřen.

Vraťte se k portálu Azure a proveďte následující kroky:

* Klikněte na "Prostředku skupiny" v levé navigační panel;
* Vyberte skupinu prostředků, která obsahuje váš virtuální počítač;
* Výsledný seznam prostředků vyberte skupinu zabezpečení sítě (jeden ikona štítu);
* Ve vlastnostech zvolte "pravidla zabezpečení příchozích";
* Na panelu nástrojů klikněte na tlačítko "Přidat";
* Zadejte název jako "výchozí povolit todo";
* Nastaví protokol na "TCP";
* Nastavit rozsah cílových portů na "8080";
* Klikněte na tlačítko OK a počkejte, který se má vytvořit pravidlo zabezpečení.

Po vytvoření tohoto pravidla zabezpečení, je aplikace TODO veřejně viditelný v síti internet a můžete procházet, například pomocí adresy URL, například:

    http://machinename.region.cloudapp.azure.com:8080

Si všimnete, že i když ještě nenakonfigurovali MongoDB virtuálního počítače, aplikace TODO se zdá být poměrně funkční. Je to proto zdrojové úložiště je pevně zakódované použití předem nasazené instance MongoDB. Jakmile jsme nakonfigurovali MongoDB virtuální počítač, bude jsme přejděte zpět a změňte zdrojový kód místo využívat naše privátní instance MongoDB.

### <a name="configuring-the-mongodb-virtual-machine"></a>Konfigurace virtuálního počítače MongoDB
SSH do druhé počítače, který jste vytvořili pomocí klienta PuTTY, ssh příkazového řádku nebo vaše jiný nástroj Oblíbené SSH. Po zobrazení uvítací zprávy a příkazového řádku, nainstalujte MongoDB (tyto pokyny, které byly odebrány z [sem](https://docs.mongodb.org/master/tutorial/install-mongodb-on-ubuntu/)):

    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv EA312927
    echo "deb http://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.2.list
    sudo apt-get update
    sudo apt-get install -y mongodb-org

Ve výchozím nastavení je nakonfigurovaný MongoDB, takže ho lze přistupovat pouze místně. V tomto kurzu jsme se nakonfigurujte MongoDB tak, aby byla přístupná z aplikace virtuálního počítače. V kontextu sudo, otevřete soubor /etc/mongod.conf a vyhledejte `# network interfaces` části. Změna `net.bindIp` hodnota konfigurace k `0.0.0.0`.

> [!NOTE]
> Tato konfigurace je pro účely tohoto kurzu jenom. Je **není** doporučené postupy zabezpečení a neměl by být používat v produkčním prostředí.
> 
> 

Teď zajistěte MongoDB byla spuštěna služba:

    sudo service mongod restart

MongoDB funguje přes port 27017 ve výchozím nastavení. Ano v stejným způsobem, který jsme potřebný pro otevření portu 8080 na virtuální počítač webového front-endu, musíme otevřete port 27017 na virtuálním počítači MongoDB.

Vraťte se k portálu Azure a proveďte následující kroky:

* Klikněte na "Prostředku skupiny" v levé navigační panel;
* Vyberte skupinu prostředků, která obsahuje virtuální počítač MongoDB;
* Výsledný seznam prostředků vyberte skupinu zabezpečení sítě (jeden ikona štítu) se stejným názvem, který jste zadali k virtuálnímu počítači MongoDB;
* Ve vlastnostech zvolte "pravidla zabezpečení příchozích";
* Na panelu nástrojů klikněte na tlačítko "Přidat";
* Zadejte název jako "výchozí povolit mongo";
* Nastaví protokol na "TCP";
* Nastavit rozsah cílových portů na "27017";
* Klikněte na tlačítko OK a počkejte, který se má vytvořit pravidlo zabezpečení.

## <a name="iterating-on-the-todo-application"></a>Iterace v aplikace TODO
Zatím jsme máte zřízen dva virtuální počítače Linux: ten, který je spuštěna aplikace front-endové webové a jeden, je spuštěna MongoDB instance. Ale došlo k potížím – front-endu webové nepoužívá ve skutečnosti zřízené instance MongoDB ještě. Umožňuje to opravíme aktualizací kód front-endu webové místo je pevně instance používala proměnné prostředí.

### <a name="changing-the-todo-application"></a>Změna aplikace TODO
Na vývojovém počítači kde nejprve klonovat úložiště uzlu todo, otevřete `node-todo/config/database.js` souboru ve svém oblíbeném editoru a změňte adresu url hodnotu z hodnoty pevně jako `mongodb://...` k `process.env.MONGODB`.

Potvrdit změny a push na Githubu hlavní server:

    git commit -am "Get MongoDB instance from env"
    git push origin master

Bohužel tuto nepodporuje publikování změnu pro virtuální počítač webového front-endu. Umožňuje provést několik více změn k tomuto virtuálnímu počítači povolit jednoduchý, ale efektivní mechanismus pro publikování aktualizací, takže můžete rychle sledovat změny v prostředí za provozu.

### <a name="configuring-the-web-frontend-virtual-machine"></a>Konfigurace virtuálního počítače webového front-endu
Odvolat jsme dříve vytvořili úplné klon todo uzlu úložiště na virtuální počítač webového front-endu. Ukazuje se, že tato akce vytvořeny nové vzdálené úložiště Git do které se můžou poslat změny. Ale vkládání jednoduše na tento vzdálený poměrně nedává rychlé iteraci modelu, který vývojářům hledáte při práci na svůj kód.

Co bychom rádi budete moct dělat je, zajistěte, aby při nabízení do vzdáleného úložiště na virtuálním počítači, běžící aplikaci TODO automaticky aktualizovala. Naštěstí jde snadno dosáhnout pomocí git.

Git udává počet háky, jež jsou volány v určitou dobu reagování na akce prováděné na úložiště. Tyto jsou určeny pomocí skriptů prostředí do úložiště `hooks` složky. Hák, který je pro tento scénář automatickou aktualizaci tehdy, je `post-update` událostí.

V relaci SSH pro virtuální počítač webového front-endu změnit na `~/node-todo.git/hooks` adresáře a přidejte následující obsah do souboru s názvem `post-update` (nahrazení `machinename` a `region` s vaše informace o virtuálním počítači MongoDB):

    #!/bin/bash

    forever stopall
    unset 'GIT_DIR'
    export MONGODB="mongodb://machinename.region.cloudapp.azure.com:27017/tododb"
    cd ~/node-todo && git fetch origin && git pull origin master && npm install && forever start ~/node-todo/server.js
    exec git update-server-info

Zkontrolujte, zda že tento soubor je spustitelný soubor tak, že spustíte následující příkaz:

    chmod 755 post-update

Tento skript zajistí, že aktuální server aplikace je zastavena, kód v úložišti klonovaný je aktualizace na nejnovější verzi, žádné aktualizované závislostí a restartování serveru. Také zajistí, že prostředí nakonfigurovaný během přípravy pro příjem naše první aktualizaci aplikace pro získání MongoDB instance z proměnné prostředí.

### <a name="configuring-your-development-machine"></a>Konfigurace počítači pro vývoj
Nyní Pojďme počítači pro vývoj připojených k virtuální počítač webového front-endu. To je jednoduché, přidání úplné úložiště na virtuálním počítači jako Vzdálená. Spusťte následující příkaz k tomu (nahrazení *uživatele* s vaše přihlašovací jméno webového front-endu virtuálního počítače a *machinename* a *oblast* podle potřeby):

    git remote add azure user@machinename.region.cloudapp.azure.com:node-todo.git

To je všechno, která je potřebná k povolení nabízení nebo publikování platit, změny virtuálního počítače webového front-endu.

### <a name="publishing-updates"></a>Publikování aktualizací
Umožňuje publikovat jeden změn, který byl změněn, pokud tak, aby aplikace bude používat vlastní instance MongoDB:

    git push azure master

Měli byste vidět výstup podobný tomuto:

    Counting objects: 4, done.
    Delta compression using up to 4 threads.
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
    To username@machinename.region.cloudapp.azure.com:node-todo.git
    5f31fd7..5bc7be5  master -> master

Po dokončení tohoto příkazu, pokuste se aktualizovat aplikaci ve webovém prohlížeči. Nyní byste měli mít zjistíte, že seznam úkolů, které jsou uvedeny v tomto tématu je prázdné a už vázanou na sdílené nasazené instance MongoDB.

K dokončení tohoto kurzu, můžeme provést jiný, více viditelných změnu. Na vývojovém počítači otevřete soubor node-todo/public/index.html pomocí vašeho oblíbeného editoru. Vyhledejte hlavičku jumbotron a změňte název z "Jsem Todo-aholic" na "Jsem aholic Todo v Azure!".

Teď umožňuje zapsat:

    git commit -am "Azurify the title"

Tento čas, budeme publikovat změny do Azure před odesláním ho do úložiště GitHub:

    git push azure master

Po dokončení tohoto příkazu, aktualizujte webové stránky a zobrazí se změny. Vzhledem k tomu, že budou vypadat dobře, push změnu zpět k počátku vzdálené: 

    git push origin master

## <a name="next-steps"></a>Další kroky
Tento článek vám ukázal, jak provést aplikace Node.js a nasadíte ho do Linuxové virtuální počítače běžící v Azure. Další informace o virtuálních počítačích s Linuxem v Azure najdete v tématu [Úvod do systému Linux v Azure](/documentation/articles/virtual-machines-linux-introduction/).

Podrobnější informace týkající se postupu při vývoji aplikací Node.js v Azure naleznete ve [Středisku pro vývojáře Node.js](/develop/nodejs/).

