---
title: "Nastavit PostgreSQL na virtuální počítač s Linuxem | Microsoft Docs"
description: "Zjistěte, jak nainstalovat a nakonfigurovat PostgreSQL na virtuální počítač s Linuxem v Azure"
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
ms.openlocfilehash: 0bccdc1cfdbda06b57da8cd662373ef137768672
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="install-and-configure-postgresql-on-azure"></a><span data-ttu-id="6446e-103">Instalace a konfigurace PostgreSQL v Azure</span><span class="sxs-lookup"><span data-stu-id="6446e-103">Install and configure PostgreSQL on Azure</span></span>
<span data-ttu-id="6446e-104">PostgreSQL je pokročilé open-source databáze podobné Oracle a DB2.</span><span class="sxs-lookup"><span data-stu-id="6446e-104">PostgreSQL is an advanced open-source database similar to Oracle and DB2.</span></span> <span data-ttu-id="6446e-105">Obsahuje funkce připravené pro organizace, například úplné ACID dodržování předpisů, spolehlivé zpracování transakcí a řízení více verzí souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="6446e-105">It includes enterprise-ready features such as full ACID compliance, reliable transactional processing, and multi-version concurrency control.</span></span> <span data-ttu-id="6446e-106">Podporuje také standardy, jako je ANSI SQL a SQL nebo MED (včetně obálky cizí dat Oracle, MySQL, MongoDB a mnoho dalších).</span><span class="sxs-lookup"><span data-stu-id="6446e-106">It also supports standards such as ANSI SQL and SQL/MED (including foreign data wrappers for Oracle, MySQL, MongoDB, and many others).</span></span> <span data-ttu-id="6446e-107">Je velmi dobře rozšiřitelná s podporou pro více než 12 procedurální jazyky, GIN a GiST indexů, podporu prostorových dat a více funkcí jako NoSQL pro JSON nebo na základě klíčů hodnota aplikace.</span><span class="sxs-lookup"><span data-stu-id="6446e-107">It is highly extensible with support for over 12 procedural languages, GIN and GiST indexes, spatial data support, and multiple NoSQL-like features for JSON or key-value-based applications.</span></span>

<span data-ttu-id="6446e-108">V tomto článku se dozvíte, jak nainstalovat a nakonfigurovat PostgreSQL na virtuální počítač Azure s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="6446e-108">In this article, you will learn how to install and configure PostgreSQL on an Azure virtual machine running Linux.</span></span>

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="install-postgresql"></a><span data-ttu-id="6446e-109">Nainstalujte PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="6446e-109">Install PostgreSQL</span></span>
> [!NOTE]
> <span data-ttu-id="6446e-110">Již musí mít virtuální počítač Azure s Linuxem k dokončení tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="6446e-110">You must already have an Azure virtual machine running Linux in order to complete this tutorial.</span></span> <span data-ttu-id="6446e-111">Vytvoření a nastavení virtuálního počítače s Linuxem než budete pokračovat, přečtěte si téma [kurzu virtuální počítač Azure s Linuxem](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="6446e-111">To create and set up a Linux VM before proceeding, see the [Azure Linux VM tutorial](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
> 
> 

<span data-ttu-id="6446e-112">V takovém případě použijte port 1999 jako PostgreSQL port.</span><span class="sxs-lookup"><span data-stu-id="6446e-112">In this case, use port 1999 as the PostgreSQL port.</span></span>  

<span data-ttu-id="6446e-113">Připojte k systému Linux vytvořené prostřednictvím PuTTY virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="6446e-113">Connect to the Linux VM you created via PuTTY.</span></span> <span data-ttu-id="6446e-114">Pokud je virtuální počítač Azure Linux používáte poprvé, přečtěte si téma [postup použití SSH se systémem Linux v Azure](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) Další informace o použití klienta PuTTY k připojení k virtuální počítač s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="6446e-114">If this is the first time you're using an Azure Linux VM, see [How to Use SSH with Linux on Azure](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) to learn how to use PuTTY to connect to a Linux VM.</span></span>

1. <span data-ttu-id="6446e-115">Spusťte následující příkaz Přepnout do kořenového adresáře (správce):</span><span class="sxs-lookup"><span data-stu-id="6446e-115">Run the following command to switch to the root (admin):</span></span>
   
        # sudo su -
2. <span data-ttu-id="6446e-116">Některé distribuce mít závislosti, které je třeba nainstalovat před instalací PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="6446e-116">Some distributions have dependencies that you must install before installing PostgreSQL.</span></span> <span data-ttu-id="6446e-117">Zkontrolujte vaše distro v tomto seznamu a spusťte příslušný příkaz:</span><span class="sxs-lookup"><span data-stu-id="6446e-117">Check for your distro in this list and run the appropriate command:</span></span>
   
   * <span data-ttu-id="6446e-118">Red Hat základní Linux:</span><span class="sxs-lookup"><span data-stu-id="6446e-118">Red Hat base Linux:</span></span>
     
           # yum install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam  libxslt-devel tcl-devel python-devel -y  
   * <span data-ttu-id="6446e-119">Debian základní Linux:</span><span class="sxs-lookup"><span data-stu-id="6446e-119">Debian base Linux:</span></span>
     
            # apt-get install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam libxslt-devel tcl-devel python-devel -y  
   * <span data-ttu-id="6446e-120">SUSE Linux:</span><span class="sxs-lookup"><span data-stu-id="6446e-120">SUSE Linux:</span></span>
     
           # zypper install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam  libxslt-devel tcl-devel python-devel -y  
3. <span data-ttu-id="6446e-121">Stáhněte PostgreSQL do kořenového adresáře a potom rozbalte balíček:</span><span class="sxs-lookup"><span data-stu-id="6446e-121">Download PostgreSQL into the root directory, and then unzip the package:</span></span>
   
        # wget https://ftp.postgresql.org/pub/source/v9.3.5/postgresql-9.3.5.tar.bz2 -P /root/
   
        # tar jxvf  postgresql-9.3.5.tar.bz2
   
    <span data-ttu-id="6446e-122">Výše je příklad.</span><span class="sxs-lookup"><span data-stu-id="6446e-122">The above is an example.</span></span> <span data-ttu-id="6446e-123">Můžete najít podrobnější adresu stahování v [Index/pub/zdroj/](https://ftp.postgresql.org/pub/source/).</span><span class="sxs-lookup"><span data-stu-id="6446e-123">You can find the more detailed download address in the [Index of /pub/source/](https://ftp.postgresql.org/pub/source/).</span></span>
4. <span data-ttu-id="6446e-124">Pokud chcete spustit sestavení, spusťte tyto příkazy:</span><span class="sxs-lookup"><span data-stu-id="6446e-124">To start the build, run these commands:</span></span>
   
        # cd postgresql-9.3.5
   
        # ./configure --prefix=/opt/postgresql-9.3.5
5. <span data-ttu-id="6446e-125">Pokud chcete vytvořit všechno, co se dají vytvářet, včetně dokumentace (stránky HTML a man) a další moduly (contrib), spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="6446e-125">If  you want to build everything that can be built, including the documentation (HTML and man pages) and additional modules (contrib), run the following command instead:</span></span>
   
        # gmake install-world
   
    <span data-ttu-id="6446e-126">Měli zobrazí následující potvrzující zpráva:</span><span class="sxs-lookup"><span data-stu-id="6446e-126">You should receive the following confirmation message:</span></span>
   
        PostgreSQL, contrib, and documentation successfully made. Ready to install.

## <a name="configure-postgresql"></a><span data-ttu-id="6446e-127">Konfigurace PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="6446e-127">Configure PostgreSQL</span></span>
1. <span data-ttu-id="6446e-128">(Volitelné) Vytvořte symbolický odkaz tak, aby zkrátil odkaz na PostgreSQL tak, aby neobsahoval číslo verze:</span><span class="sxs-lookup"><span data-stu-id="6446e-128">(Optional) Create a symbolic link to shorten the PostgreSQL reference to not include the version number:</span></span>
   
        # ln -s /opt/pgsql9.3.5 /opt/pgsql
2. <span data-ttu-id="6446e-129">Vytvořte adresář pro databázi:</span><span class="sxs-lookup"><span data-stu-id="6446e-129">Create a directory for the database:</span></span>
   
        # mkdir -p /opt/pgsql_data
3. <span data-ttu-id="6446e-130">Vytvořte uživatele nekořenovými a změna profilu uživatele.</span><span class="sxs-lookup"><span data-stu-id="6446e-130">Create a non-root user and modify that user’s profile.</span></span> <span data-ttu-id="6446e-131">Potom přepněte do tohoto nového uživatele (nazývá *postgres* v našem příkladu):</span><span class="sxs-lookup"><span data-stu-id="6446e-131">Then, switch to this new user (called *postgres* in our example):</span></span>
   
        # useradd postgres
   
        # chown -R postgres.postgres /opt/pgsql_data
   
        # su - postgres
   
   > [!NOTE]
   > <span data-ttu-id="6446e-132">Z bezpečnostních důvodů se používá PostgreSQL nekořenovými uživatele k inicializaci, spuštění nebo vypnutí databáze.</span><span class="sxs-lookup"><span data-stu-id="6446e-132">For security reasons, PostgreSQL uses a non-root user to initialize, start, or shut down the database.</span></span>
   > 
   > 
4. <span data-ttu-id="6446e-133">Upravit *bash_profile* souboru tak, že zadáte níže uvedených příkazů.</span><span class="sxs-lookup"><span data-stu-id="6446e-133">Edit the *bash_profile* file by entering the commands below.</span></span> <span data-ttu-id="6446e-134">Tyto řádky přidá na konec *bash_profile* souboru:</span><span class="sxs-lookup"><span data-stu-id="6446e-134">These lines will be added to the end of the *bash_profile* file:</span></span>
   
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
5. <span data-ttu-id="6446e-135">Spuštění *bash_profile* souboru:</span><span class="sxs-lookup"><span data-stu-id="6446e-135">Execute the *bash_profile* file:</span></span>
   
        $ source .bash_profile
6. <span data-ttu-id="6446e-136">Ověřte instalaci tak, že pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="6446e-136">Validate your installation by using the following command:</span></span>
   
        $ which psql
   
    <span data-ttu-id="6446e-137">Pokud je instalace úspěšná, zobrazí se následující odpověď:</span><span class="sxs-lookup"><span data-stu-id="6446e-137">If your installation is successful, you will see the following response:</span></span>
   
        /opt/pgsql/bin/psql
7. <span data-ttu-id="6446e-138">Můžete také zkontrolovat PostgreSQL verze:</span><span class="sxs-lookup"><span data-stu-id="6446e-138">You can also check the PostgreSQL version:</span></span>
   
        $ psql -V
8. <span data-ttu-id="6446e-139">Inicializace databáze:</span><span class="sxs-lookup"><span data-stu-id="6446e-139">Initialize the database:</span></span>
   
        $ initdb -D $PGDATA -E UTF8 --locale=C -U postgres -W
   
    <span data-ttu-id="6446e-140">Mělo by se zobrazit následující výstup:</span><span class="sxs-lookup"><span data-stu-id="6446e-140">You should receive the following output:</span></span>

![Bitové kopie](./media/postgresql-install/no1.png)

## <a name="set-up-postgresql"></a><span data-ttu-id="6446e-142">Nastavit PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="6446e-142">Set up PostgreSQL</span></span>
<!--    [postgres@ test ~]$ exit -->

<span data-ttu-id="6446e-143">Spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="6446e-143">Run the following commands:</span></span>

    # cd /root/postgresql-9.3.5/contrib/start-scripts

    # cp linux /etc/init.d/postgresql

<span data-ttu-id="6446e-144">Umožňuje změnit dvě proměnné v souboru /etc/init.d/postgresql.</span><span class="sxs-lookup"><span data-stu-id="6446e-144">Modify two variables in the /etc/init.d/postgresql file.</span></span> <span data-ttu-id="6446e-145">Předpona, která je nastavena na cestu instalace PostgreSQL: **/opt/pgsql**.</span><span class="sxs-lookup"><span data-stu-id="6446e-145">The prefix is set to the installation path of PostgreSQL: **/opt/pgsql**.</span></span> <span data-ttu-id="6446e-146">PGDATA nastavena na cestu k úložišti dat PostgreSQL: **/opt/pgsql_data**.</span><span class="sxs-lookup"><span data-stu-id="6446e-146">PGDATA is set to the data storage path of PostgreSQL: **/opt/pgsql_data**.</span></span>

    # sed -i '32s#usr/local#opt#' /etc/init.d/postgresql

    # sed -i '35s#usr/local/pgsql/data#opt/pgsql_data#' /etc/init.d/postgresql

![Bitové kopie](./media/postgresql-install/no2.png)

<span data-ttu-id="6446e-148">Změna souboru, aby spustitelný soubor:</span><span class="sxs-lookup"><span data-stu-id="6446e-148">Change the file to make it executable:</span></span>

    # chmod +x /etc/init.d/postgresql

<span data-ttu-id="6446e-149">Spusťte PostgreSQL:</span><span class="sxs-lookup"><span data-stu-id="6446e-149">Start PostgreSQL:</span></span>

    # /etc/init.d/postgresql start

<span data-ttu-id="6446e-150">Zkontrolujte, jestli koncový bod PostgreSQL na:</span><span class="sxs-lookup"><span data-stu-id="6446e-150">Check if the endpoint of PostgreSQL is on:</span></span>

    # netstat -tunlp|grep 1999

<span data-ttu-id="6446e-151">Byste měli vidět následující výstup:</span><span class="sxs-lookup"><span data-stu-id="6446e-151">You should see the following output:</span></span>

![Bitové kopie](./media/postgresql-install/no3.png)

## <a name="connect-to-the-postgres-database"></a><span data-ttu-id="6446e-153">Připojení k databázi Postgres</span><span class="sxs-lookup"><span data-stu-id="6446e-153">Connect to the Postgres database</span></span>
<span data-ttu-id="6446e-154">Přepněte na uživatele postgres znovu:</span><span class="sxs-lookup"><span data-stu-id="6446e-154">Switch to the postgres user once again:</span></span>

    # su - postgres

<span data-ttu-id="6446e-155">Vytvořte databázi Postgres:</span><span class="sxs-lookup"><span data-stu-id="6446e-155">Create a Postgres database:</span></span>

    $ createdb events

<span data-ttu-id="6446e-156">Připojení k databázi události, kterou jste právě vytvořili:</span><span class="sxs-lookup"><span data-stu-id="6446e-156">Connect to the events database that you just created:</span></span>

    $ psql -d events

## <a name="create-and-delete-a-postgres-table"></a><span data-ttu-id="6446e-157">Vytvářet a odstraňovat Postgres tabulky</span><span class="sxs-lookup"><span data-stu-id="6446e-157">Create and delete a Postgres table</span></span>
<span data-ttu-id="6446e-158">Teď, když se připojíte k databázi, můžete v něm vytvořit tabulky.</span><span class="sxs-lookup"><span data-stu-id="6446e-158">Now that you have connected to the database, you can create tables in it.</span></span>

<span data-ttu-id="6446e-159">Můžete například vytvořte novou tabulku Postgres příklad pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="6446e-159">For example, create a new example Postgres table by using the following command:</span></span>

    CREATE TABLE potluck (name VARCHAR(20),    food VARCHAR(30),    confirmed CHAR(1), signup_date DATE);

<span data-ttu-id="6446e-160">Nyní jste nastavili čtyři sloupce tabulky s následující názvy sloupců a omezení:</span><span class="sxs-lookup"><span data-stu-id="6446e-160">You have now set up a four-column table with the following column names and restrictions:</span></span>

1. <span data-ttu-id="6446e-161">Sloupec "název" omezil příkazem VARCHAR jako v části 20 znaků.</span><span class="sxs-lookup"><span data-stu-id="6446e-161">The “name” column has been limited by the VARCHAR command to be under 20 characters long.</span></span>
2. <span data-ttu-id="6446e-162">Sloupec "jídlo" označuje položku jídlo, kterými bude každá osoba.</span><span class="sxs-lookup"><span data-stu-id="6446e-162">The “food” column indicates the food item that each person will bring.</span></span> <span data-ttu-id="6446e-163">VARCHAR omezuje tento text, který má být 30 znaků.</span><span class="sxs-lookup"><span data-stu-id="6446e-163">VARCHAR limits this text to be under 30 characters.</span></span>
3. <span data-ttu-id="6446e-164">"Potvrzen" sloupec zaznamenává, jestli osoba, která má na které odpověděl společné posezení.</span><span class="sxs-lookup"><span data-stu-id="6446e-164">The “confirmed” column records whether the person has RSVP’d to the potluck.</span></span> <span data-ttu-id="6446e-165">Přípustné hodnoty jsou "Y" a "N".</span><span class="sxs-lookup"><span data-stu-id="6446e-165">The acceptable values are "Y" and "N".</span></span>
4. <span data-ttu-id="6446e-166">Zobrazuje sloupec "datum" při registraci pro událost.</span><span class="sxs-lookup"><span data-stu-id="6446e-166">The “date” column shows when they signed up for the event.</span></span> <span data-ttu-id="6446e-167">Postgres vyžaduje, aby data se zapisují jako rrrr mm-dd.</span><span class="sxs-lookup"><span data-stu-id="6446e-167">Postgres requires that dates be written as yyyy-mm-dd.</span></span>

<span data-ttu-id="6446e-168">Byste měli vidět následující, pokud tabulka byla úspěšně vytvořena:</span><span class="sxs-lookup"><span data-stu-id="6446e-168">You should see the following if your table has been successfully created:</span></span>

![Bitové kopie](./media/postgresql-install/no4.png)

<span data-ttu-id="6446e-170">Struktura tabulky můžete také zkontrolovat pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="6446e-170">You can also check the table structure by using the following command:</span></span>

![Bitové kopie](./media/postgresql-install/no5.png)

### <a name="add-data-to-a-table"></a><span data-ttu-id="6446e-172">Přidání dat do tabulky</span><span class="sxs-lookup"><span data-stu-id="6446e-172">Add data to a table</span></span>
<span data-ttu-id="6446e-173">Nejprve vložení informací do řádek:</span><span class="sxs-lookup"><span data-stu-id="6446e-173">First, insert information into a row:</span></span>

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('John', 'Casserole', 'Y', '2012-04-11');

<span data-ttu-id="6446e-174">Měli byste vidět tento výstup:</span><span class="sxs-lookup"><span data-stu-id="6446e-174">You should see this output:</span></span>

![Bitové kopie](./media/postgresql-install/no6.png)

<span data-ttu-id="6446e-176">Do tabulky také můžete přidat několik další osoby.</span><span class="sxs-lookup"><span data-stu-id="6446e-176">You can add a couple more people to the table as well.</span></span> <span data-ttu-id="6446e-177">Tady jsou některé možnosti, nebo můžete vytvořit vlastní:</span><span class="sxs-lookup"><span data-stu-id="6446e-177">Here are some options, or you can create your own:</span></span>

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('Sandy', 'Key Lime Tarts', 'N', '2012-04-14');

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES ('Tom', 'BBQ','Y', '2012-04-18');

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('Tina', 'Salad', 'Y', '2012-04-18');

### <a name="show-tables"></a><span data-ttu-id="6446e-178">Zobrazit tabulky</span><span class="sxs-lookup"><span data-stu-id="6446e-178">Show tables</span></span>
<span data-ttu-id="6446e-179">Zobrazit tabulku použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="6446e-179">Use the following command to show a table:</span></span>

    select * from potluck;

<span data-ttu-id="6446e-180">Výstup je:</span><span class="sxs-lookup"><span data-stu-id="6446e-180">The output is:</span></span>

![Bitové kopie](./media/postgresql-install/no7.png)

### <a name="delete-data-in-a-table"></a><span data-ttu-id="6446e-182">Odstranit data v tabulce</span><span class="sxs-lookup"><span data-stu-id="6446e-182">Delete data in a table</span></span>
<span data-ttu-id="6446e-183">Pokud chcete odstranit data v tabulce použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="6446e-183">Use the following command to delete data in a table:</span></span>

    delete from potluck where name=’John’;

<span data-ttu-id="6446e-184">Tím se odstraní všechny informace v řádku "Jan".</span><span class="sxs-lookup"><span data-stu-id="6446e-184">This deletes all the information in the "John" row.</span></span> <span data-ttu-id="6446e-185">Výstup je:</span><span class="sxs-lookup"><span data-stu-id="6446e-185">The output is:</span></span>

![Bitové kopie](./media/postgresql-install/no8.png)

### <a name="update-data-in-a-table"></a><span data-ttu-id="6446e-187">Aktualizovat data v tabulce</span><span class="sxs-lookup"><span data-stu-id="6446e-187">Update data in a table</span></span>
<span data-ttu-id="6446e-188">Použijte následující příkaz k aktualizaci dat v tabulce.</span><span class="sxs-lookup"><span data-stu-id="6446e-188">Use the following command to update data in a table.</span></span> <span data-ttu-id="6446e-189">Pro tento jeden Sandy potvrzuje, že se účastní, tak Změníme jeho RSVP "N" na "Y":</span><span class="sxs-lookup"><span data-stu-id="6446e-189">For this one, Sandy has confirmed that she is attending, so we will change her RSVP from "N" to "Y":</span></span>

     UPDATE potluck set confirmed = 'Y' WHERE name = 'Sandy';


## <a name="get-more-information-about-postgresql"></a><span data-ttu-id="6446e-190">Další informace o PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="6446e-190">Get more information about PostgreSQL</span></span>
<span data-ttu-id="6446e-191">Teď, když jste dokončili instalaci PostgreSQL Linux virtuální počítač Azure, můžete sledovat pomocí v Azure.</span><span class="sxs-lookup"><span data-stu-id="6446e-191">Now that you have completed the installation of PostgreSQL in an Azure Linux VM, you can enjoy using it in Azure.</span></span> <span data-ttu-id="6446e-192">Další informace o PostgreSQL, najdete [PostgreSQL webu](http://www.postgresql.org/).</span><span class="sxs-lookup"><span data-stu-id="6446e-192">To learn more about PostgreSQL, visit the [PostgreSQL website](http://www.postgresql.org/).</span></span>

