
1. tooescalate oprávněními, zadejte:
   
        sudo -s
   
    Zadejte svoje heslo.
2. tooinstall MySQL Community Server edition, zadejte:
   
        zypper install mysql-community-server
   
    Počkejte, než MySQL se stáhne a nainstaluje.
3. toostart tooset MySQL, když se spustí hello system, zadejte:
   
        insserv mysql
4. Démon MySQL hello (mysqld) spusťte ručně pomocí tohoto příkazu:
   
        rcmysql start
   
    Stav hello toocheck hello démona MySQL, zadejte:
   
        rcmysql status
   
    toostop hello démona MySQL, zadejte:
   
        rcmysql stop
   
   > [!IMPORTANT]
   > Po instalaci hello MySQL kořenové heslo je prázdné ve výchozím nastavení. Doporučujeme spustit **mysql\_zabezpečené\_instalace**, skript, který pomáhá zabezpečené MySQL. Hello skript zobrazí výzvu toochange hello MySQL kořenové heslo, odeberte anonymní uživatelské účty, zakažte kořenové vzdáleného přihlášení, odebrat testovací databáze a znovu načíst tabulku oprávnění hello. Je vhodné zodpovědět Ano tooall z těchto možností a hello kořenové heslo změnit.
   > 
   > 
5. Zadejte tento skript hello toorun MySQL instalační skript:
   
        mysql_secure_installation
6. Přihlaste se tooMySQL:
   
        mysql -u root -p
   
    Zadejte hello MySQL kořenové heslo (který jste změnili v předchozím kroku hello) a zobrazí vám s výzvou kde můžete vydat toointeract příkazy SQL s databází hello.
7. toocreate nového uživatele databáze MySQL, spusťte následující hello v hello **mysql >** řádku:
   
        CREATE USER 'mysqluser'@'localhost' IDENTIFIED BY 'password';
   
    Poznámka: hello středníkem (;) na konci hello hello řádky jsou zásadní pro ukončení hello příkazy.
8. toocreate databáze a udělte hello `mysqluser` oprávnění uživatele na něm problém hello následující příkazy:
   
        CREATE DATABASE testdatabase;
        GRANT ALL ON testdatabase.* too'mysqluser'@'localhost' IDENTIFIED BY 'password';
   
    Všimněte si, že databáze uživatelská jména a hesla používají pouze připojení databáze toohello skripty.  Názvy databáze uživatelských účtů nutně nepředstavuje skutečný uživatelské účty v systému hello.
9. toolog v z jiného počítače, zadejte:
   
        GRANT ALL ON testdatabase.* too'mysqluser'@'<ip-address>' IDENTIFIED BY 'password';
   
    kde `ip-address` je IP adresa hello hello počítače, ze kterého se připojí tooMySQL.
10. hello tooexit nástroj pro správu databáze MySQL, zadejte:
    
        quit

## <a name="add-an-endpoint"></a>Přidání koncového bodu
1. Po instalaci MySQL, budete potřebovat tooconfigure koncový bod tooaccess MySQL vzdáleně. Přihlaste se toohello [portál Azure classic][AzurePortal]. Klikněte na tlačítko **virtuální počítače**, klikněte na tlačítko hello název nového virtuálního počítače a pak klikněte na tlačítko **koncové body**.
2. Klikněte na tlačítko **přidat** na hello dolní části stránky hello.
3. Přidat koncový bod s názvem "MySQL" s protokolem **TCP**, a **veřejné** a **privátní** porty sadu příliš "3306".
4. tooremotely připojit toohello virtuálního počítače z vašeho počítače, zadejte:
   
        mysql -u mysqluser -p -h <yourservicename>.cloudapp.net
   
    Například pomocí hello virtuální počítače, který jsme vytvořili v tomto kurzu, zadejte tento příkaz:
   
        mysql -u mysqluser -p -h testlinuxvm.cloudapp.net

[MySQLDocs]: http://dev.mysql.com/doc/
[AzurePortal]: http://manage.windowsazure.com

[Image9]: ./media/install-and-run-mysql-on-opensuse-vm/LinuxVmAddEndpointMySQL.png
