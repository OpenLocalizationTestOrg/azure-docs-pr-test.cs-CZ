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
# <a name="install-and-configure-postgresql-on-azure"></a><span data-ttu-id="aa3ea-103">Instalace a konfigurace PostgreSQL v Azure</span><span class="sxs-lookup"><span data-stu-id="aa3ea-103">Install and configure PostgreSQL on Azure</span></span>
<span data-ttu-id="aa3ea-104">PostgreSQL je podobné tooOracle pokročilé databáze open source a DB2.</span><span class="sxs-lookup"><span data-stu-id="aa3ea-104">PostgreSQL is an advanced open-source database similar tooOracle and DB2.</span></span> <span data-ttu-id="aa3ea-105">Obsahuje funkce připravené pro organizace, například úplné ACID dodržování předpisů, spolehlivé zpracování transakcí a řízení více verzí souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="aa3ea-105">It includes enterprise-ready features such as full ACID compliance, reliable transactional processing, and multi-version concurrency control.</span></span> <span data-ttu-id="aa3ea-106">Podporuje také standardy, jako je ANSI SQL a SQL nebo MED (včetně obálky cizí dat Oracle, MySQL, MongoDB a mnoho dalších).</span><span class="sxs-lookup"><span data-stu-id="aa3ea-106">It also supports standards such as ANSI SQL and SQL/MED (including foreign data wrappers for Oracle, MySQL, MongoDB, and many others).</span></span> <span data-ttu-id="aa3ea-107">Je velmi dobře rozšiřitelná s podporou pro více než 12 procedurální jazyky, GIN a GiST indexů, podporu prostorových dat a více funkcí jako NoSQL pro JSON nebo na základě klíčů hodnota aplikace.</span><span class="sxs-lookup"><span data-stu-id="aa3ea-107">It is highly extensible with support for over 12 procedural languages, GIN and GiST indexes, spatial data support, and multiple NoSQL-like features for JSON or key-value-based applications.</span></span>

<span data-ttu-id="aa3ea-108">V tomto článku se dozvíte, jak tooinstall a konfigurace PostgreSQL na virtuální počítač Azure s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="aa3ea-108">In this article, you will learn how tooinstall and configure PostgreSQL on an Azure virtual machine running Linux.</span></span>

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="install-postgresql"></a><span data-ttu-id="aa3ea-109">Nainstalujte PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="aa3ea-109">Install PostgreSQL</span></span>
> [!NOTE]
> <span data-ttu-id="aa3ea-110">Již musí mít virtuální počítač Azure s Linuxem v toocomplete pořadí v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="aa3ea-110">You must already have an Azure virtual machine running Linux in order toocomplete this tutorial.</span></span> <span data-ttu-id="aa3ea-111">toocreate a nastavení virtuálního počítače s Linuxem než budete pokračovat, najdete v článku [kurzu virtuální počítač Azure s Linuxem](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="aa3ea-111">toocreate and set up a Linux VM before proceeding, see the [Azure Linux VM tutorial](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
> 
> 

<span data-ttu-id="aa3ea-112">V takovém případě port 1999 použijte jako hello PostgreSQL portu.</span><span class="sxs-lookup"><span data-stu-id="aa3ea-112">In this case, use port 1999 as hello PostgreSQL port.</span></span>  

<span data-ttu-id="aa3ea-113">Připojte toohello vytvořené prostřednictvím PuTTY virtuálního počítače s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="aa3ea-113">Connect toohello Linux VM you created via PuTTY.</span></span> <span data-ttu-id="aa3ea-114">Pokud je to hello poprvé používáte Linux virtuálního počítače Azure, najdete v části [jak tooUse SSH s Linuxem v Azure](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) toolearn jak toouse PuTTY tooconnect tooa virtuálního počítače s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="aa3ea-114">If this is hello first time you're using an Azure Linux VM, see [How tooUse SSH with Linux on Azure](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) toolearn how toouse PuTTY tooconnect tooa Linux VM.</span></span>

1. <span data-ttu-id="aa3ea-115">Spusťte následující příkaz tooswitch toohello kořenové (správce) hello:</span><span class="sxs-lookup"><span data-stu-id="aa3ea-115">Run hello following command tooswitch toohello root (admin):</span></span>
   
        # sudo su -
2. <span data-ttu-id="aa3ea-116">Některé distribuce mít závislosti, které je třeba nainstalovat před instalací PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="aa3ea-116">Some distributions have dependencies that you must install before installing PostgreSQL.</span></span> <span data-ttu-id="aa3ea-117">Zkontrolujte vaše distro v tomto seznamu a spusťte příslušný příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="aa3ea-117">Check for your distro in this list and run hello appropriate command:</span></span>
   
   * <span data-ttu-id="aa3ea-118">Red Hat základní Linux:</span><span class="sxs-lookup"><span data-stu-id="aa3ea-118">Red Hat base Linux:</span></span>
     
           # yum install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam  libxslt-devel tcl-devel python-devel -y  
   * <span data-ttu-id="aa3ea-119">Debian základní Linux:</span><span class="sxs-lookup"><span data-stu-id="aa3ea-119">Debian base Linux:</span></span>
     
            # apt-get install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam libxslt-devel tcl-devel python-devel -y  
   * <span data-ttu-id="aa3ea-120">SUSE Linux:</span><span class="sxs-lookup"><span data-stu-id="aa3ea-120">SUSE Linux:</span></span>
     
           # zypper install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam  libxslt-devel tcl-devel python-devel -y  
3. <span data-ttu-id="aa3ea-121">Stáhněte PostgreSQL do hello kořenový adresář a potom rozbalte balíček hello:</span><span class="sxs-lookup"><span data-stu-id="aa3ea-121">Download PostgreSQL into hello root directory, and then unzip hello package:</span></span>
   
        # wget https://ftp.postgresql.org/pub/source/v9.3.5/postgresql-9.3.5.tar.bz2 -P /root/
   
        # tar jxvf  postgresql-9.3.5.tar.bz2
   
    <span data-ttu-id="aa3ea-122">Hello výše je příklad.</span><span class="sxs-lookup"><span data-stu-id="aa3ea-122">hello above is an example.</span></span> <span data-ttu-id="aa3ea-123">Můžete najít hello podrobnější stáhnout adresu v hello [Index/pub/zdroj/](https://ftp.postgresql.org/pub/source/).</span><span class="sxs-lookup"><span data-stu-id="aa3ea-123">You can find hello more detailed download address in hello [Index of /pub/source/](https://ftp.postgresql.org/pub/source/).</span></span>
4. <span data-ttu-id="aa3ea-124">toostart hello sestavení, spusťte tyto příkazy:</span><span class="sxs-lookup"><span data-stu-id="aa3ea-124">toostart hello build, run these commands:</span></span>
   
        # cd postgresql-9.3.5
   
        # ./configure --prefix=/opt/postgresql-9.3.5
5. <span data-ttu-id="aa3ea-125">Pokud chcete, aby toobuild všechno, co se dají vytvářet, včetně dokumentace hello (stránky HTML a man) a další moduly (contrib), spusťte následující příkaz místo hello:</span><span class="sxs-lookup"><span data-stu-id="aa3ea-125">If  you want toobuild everything that can be built, including hello documentation (HTML and man pages) and additional modules (contrib), run hello following command instead:</span></span>
   
        # gmake install-world
   
    <span data-ttu-id="aa3ea-126">Měli byste obdržet hello následující potvrzující zpráva:</span><span class="sxs-lookup"><span data-stu-id="aa3ea-126">You should receive hello following confirmation message:</span></span>
   
        PostgreSQL, contrib, and documentation successfully made. Ready tooinstall.

## <a name="configure-postgresql"></a><span data-ttu-id="aa3ea-127">Konfigurace PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="aa3ea-127">Configure PostgreSQL</span></span>
1. <span data-ttu-id="aa3ea-128">(Volitelné) Vytvoření symbolický odkaz tooshorten hello PostgreSQL odkaz toonot zahrnují hello číslo verze:</span><span class="sxs-lookup"><span data-stu-id="aa3ea-128">(Optional) Create a symbolic link tooshorten hello PostgreSQL reference toonot include hello version number:</span></span>
   
        # ln -s /opt/pgsql9.3.5 /opt/pgsql
2. <span data-ttu-id="aa3ea-129">Vytvořte adresář pro databázi hello:</span><span class="sxs-lookup"><span data-stu-id="aa3ea-129">Create a directory for hello database:</span></span>
   
        # mkdir -p /opt/pgsql_data
3. <span data-ttu-id="aa3ea-130">Vytvořte uživatele nekořenovými a změna profilu uživatele.</span><span class="sxs-lookup"><span data-stu-id="aa3ea-130">Create a non-root user and modify that user’s profile.</span></span> <span data-ttu-id="aa3ea-131">Potom přepněte nový uživatel toothis (nazývá *postgres* v našem příkladu):</span><span class="sxs-lookup"><span data-stu-id="aa3ea-131">Then, switch toothis new user (called *postgres* in our example):</span></span>
   
        # useradd postgres
   
        # chown -R postgres.postgres /opt/pgsql_data
   
        # su - postgres
   
   > [!NOTE]
   > <span data-ttu-id="aa3ea-132">Z bezpečnostních důvodů se používá PostgreSQL tooinitialize bez kořenového uživatele, spuštění nebo vypnutí databáze hello.</span><span class="sxs-lookup"><span data-stu-id="aa3ea-132">For security reasons, PostgreSQL uses a non-root user tooinitialize, start, or shut down hello database.</span></span>
   > 
   > 
4. <span data-ttu-id="aa3ea-133">Upravit hello *bash_profile* souboru tak, že zadáte níže uvedené příkazy hello.</span><span class="sxs-lookup"><span data-stu-id="aa3ea-133">Edit hello *bash_profile* file by entering hello commands below.</span></span> <span data-ttu-id="aa3ea-134">Tyto řádky budou přidány toohello konec hello *bash_profile* souboru:</span><span class="sxs-lookup"><span data-stu-id="aa3ea-134">These lines will be added toohello end of hello *bash_profile* file:</span></span>
   
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
5. <span data-ttu-id="aa3ea-135">Spuštění hello *bash_profile* souboru:</span><span class="sxs-lookup"><span data-stu-id="aa3ea-135">Execute hello *bash_profile* file:</span></span>
   
        $ source .bash_profile
6. <span data-ttu-id="aa3ea-136">Ověřte instalaci hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="aa3ea-136">Validate your installation by using hello following command:</span></span>
   
        $ which psql
   
    <span data-ttu-id="aa3ea-137">Pokud je instalace úspěšná, zobrazí se následující odpověď hello:</span><span class="sxs-lookup"><span data-stu-id="aa3ea-137">If your installation is successful, you will see hello following response:</span></span>
   
        /opt/pgsql/bin/psql
7. <span data-ttu-id="aa3ea-138">Můžete také zkontrolovat hello PostgreSQL verze:</span><span class="sxs-lookup"><span data-stu-id="aa3ea-138">You can also check hello PostgreSQL version:</span></span>
   
        $ psql -V
8. <span data-ttu-id="aa3ea-139">Inicializace hello databáze:</span><span class="sxs-lookup"><span data-stu-id="aa3ea-139">Initialize hello database:</span></span>
   
        $ initdb -D $PGDATA -E UTF8 --locale=C -U postgres -W
   
    <span data-ttu-id="aa3ea-140">Měli byste obdržet hello následující výstup:</span><span class="sxs-lookup"><span data-stu-id="aa3ea-140">You should receive hello following output:</span></span>

![Bitové kopie](./media/postgresql-install/no1.png)

## <a name="set-up-postgresql"></a><span data-ttu-id="aa3ea-142">Nastavit PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="aa3ea-142">Set up PostgreSQL</span></span>
<!--    [postgres@ test ~]$ exit -->

<span data-ttu-id="aa3ea-143">Spusťte následující příkazy hello:</span><span class="sxs-lookup"><span data-stu-id="aa3ea-143">Run hello following commands:</span></span>

    # cd /root/postgresql-9.3.5/contrib/start-scripts

    # cp linux /etc/init.d/postgresql

<span data-ttu-id="aa3ea-144">Umožňuje změnit dvě proměnné v souboru /etc/init.d/postgresql hello.</span><span class="sxs-lookup"><span data-stu-id="aa3ea-144">Modify two variables in hello /etc/init.d/postgresql file.</span></span> <span data-ttu-id="aa3ea-145">Předpona Hello nastavena cesta instalace toohello PostgreSQL: **/opt/pgsql**.</span><span class="sxs-lookup"><span data-stu-id="aa3ea-145">hello prefix is set toohello installation path of PostgreSQL: **/opt/pgsql**.</span></span> <span data-ttu-id="aa3ea-146">PGDATA je nastavit cestu k úložišti dat toohello z PostgreSQL: **/opt/pgsql_data**.</span><span class="sxs-lookup"><span data-stu-id="aa3ea-146">PGDATA is set toohello data storage path of PostgreSQL: **/opt/pgsql_data**.</span></span>

    # sed -i '32s#usr/local#opt#' /etc/init.d/postgresql

    # sed -i '35s#usr/local/pgsql/data#opt/pgsql_data#' /etc/init.d/postgresql

![Bitové kopie](./media/postgresql-install/no2.png)

<span data-ttu-id="aa3ea-148">Změna souboru toomake hello je spustitelný soubor:</span><span class="sxs-lookup"><span data-stu-id="aa3ea-148">Change hello file toomake it executable:</span></span>

    # chmod +x /etc/init.d/postgresql

<span data-ttu-id="aa3ea-149">Spusťte PostgreSQL:</span><span class="sxs-lookup"><span data-stu-id="aa3ea-149">Start PostgreSQL:</span></span>

    # /etc/init.d/postgresql start

<span data-ttu-id="aa3ea-150">Zkontrolujte, jestli koncový bod hello PostgreSQL na:</span><span class="sxs-lookup"><span data-stu-id="aa3ea-150">Check if hello endpoint of PostgreSQL is on:</span></span>

    # netstat -tunlp|grep 1999

<span data-ttu-id="aa3ea-151">Měli byste vidět hello následující výstup:</span><span class="sxs-lookup"><span data-stu-id="aa3ea-151">You should see hello following output:</span></span>

![Bitové kopie](./media/postgresql-install/no3.png)

## <a name="connect-toohello-postgres-database"></a><span data-ttu-id="aa3ea-153">Připojit databáze Postgres toohello</span><span class="sxs-lookup"><span data-stu-id="aa3ea-153">Connect toohello Postgres database</span></span>
<span data-ttu-id="aa3ea-154">Přepněte uživatele postgres toohello znovu:</span><span class="sxs-lookup"><span data-stu-id="aa3ea-154">Switch toohello postgres user once again:</span></span>

    # su - postgres

<span data-ttu-id="aa3ea-155">Vytvořte databázi Postgres:</span><span class="sxs-lookup"><span data-stu-id="aa3ea-155">Create a Postgres database:</span></span>

    $ createdb events

<span data-ttu-id="aa3ea-156">Připojte databáze toohello události, kterou jste právě vytvořili:</span><span class="sxs-lookup"><span data-stu-id="aa3ea-156">Connect toohello events database that you just created:</span></span>

    $ psql -d events

## <a name="create-and-delete-a-postgres-table"></a><span data-ttu-id="aa3ea-157">Vytvářet a odstraňovat Postgres tabulky</span><span class="sxs-lookup"><span data-stu-id="aa3ea-157">Create and delete a Postgres table</span></span>
<span data-ttu-id="aa3ea-158">Teď, když připojíte toohello databáze, můžete v něm vytvořit tabulky.</span><span class="sxs-lookup"><span data-stu-id="aa3ea-158">Now that you have connected toohello database, you can create tables in it.</span></span>

<span data-ttu-id="aa3ea-159">Můžete například vytvořte novou tabulku Postgres příklad pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="aa3ea-159">For example, create a new example Postgres table by using hello following command:</span></span>

    CREATE TABLE potluck (name VARCHAR(20),    food VARCHAR(30),    confirmed CHAR(1), signup_date DATE);

<span data-ttu-id="aa3ea-160">Nyní jste nastavili tak čtyři sloupce tabulky s hello následující názvy sloupců a omezení:</span><span class="sxs-lookup"><span data-stu-id="aa3ea-160">You have now set up a four-column table with hello following column names and restrictions:</span></span>

1. <span data-ttu-id="aa3ea-161">Hello "name" sloupec má byla omezena hello VARCHAR příkaz toobe méně než 20 znaků.</span><span class="sxs-lookup"><span data-stu-id="aa3ea-161">hello “name” column has been limited by hello VARCHAR command toobe under 20 characters long.</span></span>
2. <span data-ttu-id="aa3ea-162">sloupec "jídlo" Hello označuje položku jídlo hello, kterými se každá osoba.</span><span class="sxs-lookup"><span data-stu-id="aa3ea-162">hello “food” column indicates hello food item that each person will bring.</span></span> <span data-ttu-id="aa3ea-163">VARCHAR omezuje tento text toobe pod 30 znaků.</span><span class="sxs-lookup"><span data-stu-id="aa3ea-163">VARCHAR limits this text toobe under 30 characters.</span></span>
3. <span data-ttu-id="aa3ea-164">Hello "Potvrdit" sloupec záznamů jestli osoba hello má na které odpověděl společné posezení toohello.</span><span class="sxs-lookup"><span data-stu-id="aa3ea-164">hello “confirmed” column records whether hello person has RSVP’d toohello potluck.</span></span> <span data-ttu-id="aa3ea-165">Hello přípustné hodnoty jsou "Y" a "N".</span><span class="sxs-lookup"><span data-stu-id="aa3ea-165">hello acceptable values are "Y" and "N".</span></span>
4. <span data-ttu-id="aa3ea-166">Zobrazuje sloupec datum"Hello", při registraci pro událost hello.</span><span class="sxs-lookup"><span data-stu-id="aa3ea-166">hello “date” column shows when they signed up for hello event.</span></span> <span data-ttu-id="aa3ea-167">Postgres vyžaduje, aby data se zapisují jako rrrr mm-dd.</span><span class="sxs-lookup"><span data-stu-id="aa3ea-167">Postgres requires that dates be written as yyyy-mm-dd.</span></span>

<span data-ttu-id="aa3ea-168">Pokud vaše tabulka byla úspěšně vytvořena, měli byste vidět hello následující:</span><span class="sxs-lookup"><span data-stu-id="aa3ea-168">You should see hello following if your table has been successfully created:</span></span>

![Bitové kopie](./media/postgresql-install/no4.png)

<span data-ttu-id="aa3ea-170">Struktura tabulky hello můžete také zkontrolovat pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="aa3ea-170">You can also check hello table structure by using hello following command:</span></span>

![Bitové kopie](./media/postgresql-install/no5.png)

### <a name="add-data-tooa-table"></a><span data-ttu-id="aa3ea-172">Přidat tabulku tooa dat</span><span class="sxs-lookup"><span data-stu-id="aa3ea-172">Add data tooa table</span></span>
<span data-ttu-id="aa3ea-173">Nejprve vložení informací do řádek:</span><span class="sxs-lookup"><span data-stu-id="aa3ea-173">First, insert information into a row:</span></span>

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('John', 'Casserole', 'Y', '2012-04-11');

<span data-ttu-id="aa3ea-174">Měli byste vidět tento výstup:</span><span class="sxs-lookup"><span data-stu-id="aa3ea-174">You should see this output:</span></span>

![Bitové kopie](./media/postgresql-install/no6.png)

<span data-ttu-id="aa3ea-176">Můžete přidat několik další osoby toohello tabulce také.</span><span class="sxs-lookup"><span data-stu-id="aa3ea-176">You can add a couple more people toohello table as well.</span></span> <span data-ttu-id="aa3ea-177">Tady jsou některé možnosti, nebo můžete vytvořit vlastní:</span><span class="sxs-lookup"><span data-stu-id="aa3ea-177">Here are some options, or you can create your own:</span></span>

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('Sandy', 'Key Lime Tarts', 'N', '2012-04-14');

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES ('Tom', 'BBQ','Y', '2012-04-18');

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('Tina', 'Salad', 'Y', '2012-04-18');

### <a name="show-tables"></a><span data-ttu-id="aa3ea-178">Zobrazit tabulky</span><span class="sxs-lookup"><span data-stu-id="aa3ea-178">Show tables</span></span>
<span data-ttu-id="aa3ea-179">Použijte následující příkaz tooshow tabulku hello:</span><span class="sxs-lookup"><span data-stu-id="aa3ea-179">Use hello following command tooshow a table:</span></span>

    select * from potluck;

<span data-ttu-id="aa3ea-180">výstup Hello je:</span><span class="sxs-lookup"><span data-stu-id="aa3ea-180">hello output is:</span></span>

![Bitové kopie](./media/postgresql-install/no7.png)

### <a name="delete-data-in-a-table"></a><span data-ttu-id="aa3ea-182">Odstranit data v tabulce</span><span class="sxs-lookup"><span data-stu-id="aa3ea-182">Delete data in a table</span></span>
<span data-ttu-id="aa3ea-183">Použijte následující příkaz toodelete data v tabulce hello:</span><span class="sxs-lookup"><span data-stu-id="aa3ea-183">Use hello following command toodelete data in a table:</span></span>

    delete from potluck where name=’John’;

<span data-ttu-id="aa3ea-184">Tím se odstraní všechny informace hello v hello "Jan" řádek.</span><span class="sxs-lookup"><span data-stu-id="aa3ea-184">This deletes all hello information in hello "John" row.</span></span> <span data-ttu-id="aa3ea-185">výstup Hello je:</span><span class="sxs-lookup"><span data-stu-id="aa3ea-185">hello output is:</span></span>

![Bitové kopie](./media/postgresql-install/no8.png)

### <a name="update-data-in-a-table"></a><span data-ttu-id="aa3ea-187">Aktualizovat data v tabulce</span><span class="sxs-lookup"><span data-stu-id="aa3ea-187">Update data in a table</span></span>
<span data-ttu-id="aa3ea-188">Použijte následující příkaz tooupdate data v tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="aa3ea-188">Use hello following command tooupdate data in a table.</span></span> <span data-ttu-id="aa3ea-189">Pro tento jeden, Sandy potvrzuje, že se účastní, tak Změníme jeho zasílání zpráv rysy "N" příliš "Y":</span><span class="sxs-lookup"><span data-stu-id="aa3ea-189">For this one, Sandy has confirmed that she is attending, so we will change her RSVP from "N" too"Y":</span></span>

     UPDATE potluck set confirmed = 'Y' WHERE name = 'Sandy';


## <a name="get-more-information-about-postgresql"></a><span data-ttu-id="aa3ea-190">Další informace o PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="aa3ea-190">Get more information about PostgreSQL</span></span>
<span data-ttu-id="aa3ea-191">Teď, když jste dokončili instalaci hello PostgreSQL Linux virtuální počítač Azure, můžete sledovat pomocí v Azure.</span><span class="sxs-lookup"><span data-stu-id="aa3ea-191">Now that you have completed hello installation of PostgreSQL in an Azure Linux VM, you can enjoy using it in Azure.</span></span> <span data-ttu-id="aa3ea-192">toolearn Další informace o PostgreSQL, navštivte hello [PostgreSQL webu](http://www.postgresql.org/).</span><span class="sxs-lookup"><span data-stu-id="aa3ea-192">toolearn more about PostgreSQL, visit hello [PostgreSQL website](http://www.postgresql.org/).</span></span>

