---
title: "aaaSet až PostgreSQL na virtuální počítač s Linuxem | Microsoft Docs"
description: "Zjistěte, jak tooinstall a konfigurace PostgreSQL na virtuální počítač s Linuxem v Azure"
services: virtual-machines-linux
documentationcenter: 
author: SuperScottz
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 1a747363-0cc5-4ba3-9be7-084dfeb04651
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/01/2016
ms.author: mingzhan
ms.openlocfilehash: 40209647924dffce11500705eb2d9f41c14df6ab
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="install-and-configure-postgresql-on-azure"></a>Instalace a konfigurace PostgreSQL v Azure
PostgreSQL je podobné tooOracle pokročilé databáze open source a DB2. Obsahuje funkce připravené pro organizace, například úplné ACID dodržování předpisů, spolehlivé zpracování transakcí a řízení více verzí souběžnosti. Podporuje také standardy, jako je ANSI SQL a SQL nebo MED (včetně obálky cizí dat Oracle, MySQL, MongoDB a mnoho dalších). Je velmi dobře rozšiřitelná s podporou pro více než 12 procedurální jazyky, GIN a GiST indexů, podporu prostorových dat a více funkcí jako NoSQL pro JSON nebo na základě klíčů hodnota aplikace.

V tomto článku se dozvíte, jak tooinstall a konfigurace PostgreSQL na virtuální počítač Azure s Linuxem.

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="install-postgresql"></a>Nainstalujte PostgreSQL
> [!NOTE]
> Již musí mít virtuální počítač Azure s Linuxem v toocomplete pořadí v tomto kurzu. toocreate a nastavení virtuálního počítače s Linuxem než budete pokračovat, najdete v článku [kurzu virtuální počítač Azure s Linuxem](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
> 
> 

V takovém případě port 1999 použijte jako hello PostgreSQL portu.  

Připojte toohello vytvořené prostřednictvím PuTTY virtuálního počítače s Linuxem. Pokud je to hello poprvé používáte Linux virtuálního počítače Azure, najdete v části [jak tooUse SSH s Linuxem v Azure](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) toolearn jak toouse PuTTY tooconnect tooa virtuálního počítače s Linuxem.

1. Spusťte následující příkaz tooswitch toohello kořenové (správce) hello:
   
        # sudo su -
2. Některé distribuce mít závislosti, které je třeba nainstalovat před instalací PostgreSQL. Zkontrolujte vaše distro v tomto seznamu a spusťte příslušný příkaz hello:
   
   * Red Hat základní Linux:
     
           # yum install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam  libxslt-devel tcl-devel python-devel -y  
   * Debian základní Linux:
     
            # apt-get install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam libxslt-devel tcl-devel python-devel -y  
   * SUSE Linux:
     
           # zypper install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam  libxslt-devel tcl-devel python-devel -y  
3. Stáhněte PostgreSQL do hello kořenový adresář a potom rozbalte balíček hello:
   
        # wget https://ftp.postgresql.org/pub/source/v9.3.5/postgresql-9.3.5.tar.bz2 -P /root/
   
        # tar jxvf  postgresql-9.3.5.tar.bz2
   
    Hello výše je příklad. Můžete najít hello podrobnější stáhnout adresu v hello [Index/pub/zdroj/](https://ftp.postgresql.org/pub/source/).
4. toostart hello sestavení, spusťte tyto příkazy:
   
        # cd postgresql-9.3.5
   
        # ./configure --prefix=/opt/postgresql-9.3.5
5. Pokud chcete, aby toobuild všechno, co se dají vytvářet, včetně dokumentace hello (stránky HTML a man) a další moduly (contrib), spusťte následující příkaz místo hello:
   
        # gmake install-world
   
    Měli byste obdržet hello následující potvrzující zpráva:
   
        PostgreSQL, contrib, and documentation successfully made. Ready tooinstall.

## <a name="configure-postgresql"></a>Konfigurace PostgreSQL
1. (Volitelné) Vytvoření symbolický odkaz tooshorten hello PostgreSQL odkaz toonot zahrnují hello číslo verze:
   
        # ln -s /opt/pgsql9.3.5 /opt/pgsql
2. Vytvořte adresář pro databázi hello:
   
        # mkdir -p /opt/pgsql_data
3. Vytvořte uživatele nekořenovými a změna profilu uživatele. Potom přepněte nový uživatel toothis (nazývá *postgres* v našem příkladu):
   
        # useradd postgres
   
        # chown -R postgres.postgres /opt/pgsql_data
   
        # su - postgres
   
   > [!NOTE]
   > Z bezpečnostních důvodů se používá PostgreSQL tooinitialize bez kořenového uživatele, spuštění nebo vypnutí databáze hello.
   > 
   > 
4. Upravit hello *bash_profile* souboru tak, že zadáte níže uvedené příkazy hello. Tyto řádky budou přidány toohello konec hello *bash_profile* souboru:
   
        cat >> ~/.bash_profile <<EOF
        export PGPORT=1999
        export PGDATA=/opt/pgsql_data
        export LANG=en_US.utf8
        export PGHOME=/opt/pgsql
        export PATH=\$PATH:\$PGHOME/bin
        export MANPATH=\$MANPATH:\$PGHOME/share/man
        export DATA=`date +"%Y%m%d%H%M"`
        export PGUSER=postgres
        alias rm='rm -i'
        alias ll='ls -lh'
        EOF
5. Spuštění hello *bash_profile* souboru:
   
        $ source .bash_profile
6. Ověřte instalaci hello následující příkaz:
   
        $ which psql
   
    Pokud je instalace úspěšná, zobrazí se následující odpověď hello:
   
        /opt/pgsql/bin/psql
7. Můžete také zkontrolovat hello PostgreSQL verze:
   
        $ psql -V
8. Inicializace hello databáze:
   
        $ initdb -D $PGDATA -E UTF8 --locale=C -U postgres -W
   
    Měli byste obdržet hello následující výstup:

![Bitové kopie](./media/postgresql-install/no1.png)

## <a name="set-up-postgresql"></a>Nastavit PostgreSQL
<!--    [postgres@ test ~]$ exit -->

Spusťte následující příkazy hello:

    # cd /root/postgresql-9.3.5/contrib/start-scripts

    # cp linux /etc/init.d/postgresql

Umožňuje změnit dvě proměnné v souboru /etc/init.d/postgresql hello. Předpona Hello nastavena cesta instalace toohello PostgreSQL: **/opt/pgsql**. PGDATA je nastavit cestu k úložišti dat toohello z PostgreSQL: **/opt/pgsql_data**.

    # sed -i '32s#usr/local#opt#' /etc/init.d/postgresql

    # sed -i '35s#usr/local/pgsql/data#opt/pgsql_data#' /etc/init.d/postgresql

![Bitové kopie](./media/postgresql-install/no2.png)

Změna souboru toomake hello je spustitelný soubor:

    # chmod +x /etc/init.d/postgresql

Spusťte PostgreSQL:

    # /etc/init.d/postgresql start

Zkontrolujte, jestli koncový bod hello PostgreSQL na:

    # netstat -tunlp|grep 1999

Měli byste vidět hello následující výstup:

![Bitové kopie](./media/postgresql-install/no3.png)

## <a name="connect-toohello-postgres-database"></a>Připojit databáze Postgres toohello
Přepněte uživatele postgres toohello znovu:

    # su - postgres

Vytvořte databázi Postgres:

    $ createdb events

Připojte databáze toohello události, kterou jste právě vytvořili:

    $ psql -d events

## <a name="create-and-delete-a-postgres-table"></a>Vytvářet a odstraňovat Postgres tabulky
Teď, když připojíte toohello databáze, můžete v něm vytvořit tabulky.

Můžete například vytvořte novou tabulku Postgres příklad pomocí hello následující příkaz:

    CREATE TABLE potluck (name VARCHAR(20),    food VARCHAR(30),    confirmed CHAR(1), signup_date DATE);

Nyní jste nastavili tak čtyři sloupce tabulky s hello následující názvy sloupců a omezení:

1. Hello "name" sloupec má byla omezena hello VARCHAR příkaz toobe méně než 20 znaků.
2. sloupec "jídlo" Hello označuje položku jídlo hello, kterými se každá osoba. VARCHAR omezuje tento text toobe pod 30 znaků.
3. Hello "Potvrdit" sloupec záznamů jestli osoba hello má na které odpověděl společné posezení toohello. Hello přípustné hodnoty jsou "Y" a "N".
4. Zobrazuje sloupec datum"Hello", při registraci pro událost hello. Postgres vyžaduje, aby data se zapisují jako rrrr mm-dd.

Pokud vaše tabulka byla úspěšně vytvořena, měli byste vidět hello následující:

![Bitové kopie](./media/postgresql-install/no4.png)

Struktura tabulky hello můžete také zkontrolovat pomocí hello následující příkaz:

![Bitové kopie](./media/postgresql-install/no5.png)

### <a name="add-data-tooa-table"></a>Přidat tabulku tooa dat
Nejprve vložení informací do řádek:

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('John', 'Casserole', 'Y', '2012-04-11');

Měli byste vidět tento výstup:

![Bitové kopie](./media/postgresql-install/no6.png)

Můžete přidat několik další osoby toohello tabulce také. Tady jsou některé možnosti, nebo můžete vytvořit vlastní:

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('Sandy', 'Key Lime Tarts', 'N', '2012-04-14');

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES ('Tom', 'BBQ','Y', '2012-04-18');

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('Tina', 'Salad', 'Y', '2012-04-18');

### <a name="show-tables"></a>Zobrazit tabulky
Použijte následující příkaz tooshow tabulku hello:

    select * from potluck;

výstup Hello je:

![Bitové kopie](./media/postgresql-install/no7.png)

### <a name="delete-data-in-a-table"></a>Odstranit data v tabulce
Použijte následující příkaz toodelete data v tabulce hello:

    delete from potluck where name=’John’;

Tím se odstraní všechny informace hello v hello "Jan" řádek. výstup Hello je:

![Bitové kopie](./media/postgresql-install/no8.png)

### <a name="update-data-in-a-table"></a>Aktualizovat data v tabulce
Použijte následující příkaz tooupdate data v tabulce hello. Pro tento jeden, Sandy potvrzuje, že se účastní, tak Změníme jeho zasílání zpráv rysy "N" příliš "Y":

     UPDATE potluck set confirmed = 'Y' WHERE name = 'Sandy';


## <a name="get-more-information-about-postgresql"></a>Další informace o PostgreSQL
Teď, když jste dokončili instalaci hello PostgreSQL Linux virtuální počítač Azure, můžete sledovat pomocí v Azure. toolearn Další informace o PostgreSQL, navštivte hello [PostgreSQL webu](http://www.postgresql.org/).

