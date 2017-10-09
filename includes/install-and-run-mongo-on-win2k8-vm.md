Postupujte podle těchto kroků tooinstall a spusťte MongoDB virtuálního počítače se systémem Windows Server.

> [!IMPORTANT]
> Funkce zabezpečení MongoDB, jako je ověřování a vazbu IP adresy, nejsou povolené ve výchozím nastavení. Před nasazením MongoDB tooa produkčním prostředí by měl povolit funkce zabezpečení.  Další informace najdete v tématu [zabezpečení a ověřování](http://www.mongodb.org/display/DOCS/Security+and+Authentication).
>
>

1. Po připojení toohello virtuálního počítače pomocí vzdálené plochy, otevřete Internet Explorer z hello **spustit** nabídky hello virtuálního počítače.
2. Vyberte hello **nástroje** tlačítko v pravém horním rohu hello.  V **Možnosti Internetu**, vyberte hello **zabezpečení** a pak vyberte hello **Důvěryhodné servery** ikonu a nakonec klikněte na tlačítko hello **lokality** tlačítko. Přidat *https://\*. mongodb.org* toohello seznam důvěryhodných serverů.
3. Přejděte příliš[stáhne - MongoDB](https://www.mongodb.com/download-center#community).
4. Najde hello **aktuální stabilní verzi** z **komunity serveru**, vyberte nejnovější hello **64-bit** verze ve sloupci Windows hello. Stáhněte a spusťte instalační program MSI hello.
5. MongoDB je obvykle nainstalován v C:\Program Files\MongoDB. Vyhledejte proměnné prostředí na ploše hello a přidejte hello MongoDB binární soubory cesta toohello proměnné PATH. Například můžete zjistit hello binárních souborů v C:\Program Files\MongoDB\Server\3.4\bin na váš počítač.
6. Vytváření adresářů MongoDB protokolu a data v hello datový disk (například disk **F:**) jste vytvořili v předchozích krocích hello. Z **spustit**, vyberte **příkazového řádku** tooopen okno příkazového řádku.  Zadejte:

        C:\> F:
        F:\> mkdir \MongoData
        F:\> mkdir \MongoLogs
7. toorun hello databáze, spusťte:

        F:\> C:
        C:\> mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log

    Všechny zprávy protokolu jsou řízené toohello *F:\MongoLogs\mongolog.log* souborů jako mongod.exe server spustí a preallocates soubory deníku. Může trvat několik minut, než MongoDB toopreallocate soubory deníku hello a zahájit naslouchání pro připojení. příkazový řádek Hello zůstává zaměřují se na tato úloha je spuštěna MongoDB instance.
8. hello toostart MongoDB prostředí pro správu, otevřete další okno příkaz z **spustit** a typ hello následující příkazy:

        C:\> cd \my_mongo_dir\bin  
        C:\my_mongo_dir\bin> mongo  
        >db  
        test
        > db.foo.insert( { a : 1 } )  
        > db.foo.find()  
        { _id : ..., a : 1 }  
        > show dbs  
        ...  
        > show collections  
        ...  
        > help  

    Hello databáze byla vytvořena ve hello insert.
9. Alternativně můžete nainstalovat mongod.exe jako služba:

        C:\> mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log --logappend  --install

    Je nainstalována služba s názvem MongoDB s popisem "Mongo DB". Hello `--logpath` možnost musí být použité toospecify soubor protokolu, od hello spuštěna služba nemá na příkaz okna toodisplay výstup.  Hello `--logappend` možnost určuje, že restartování služby hello způsobí, že výstupní tooappend toohello existující soubor protokolu.  Hello `--dbpath` možnost určuje umístění hello hello dat adresáře. Další související se službou možnosti příkazového řádku najdete v tématu [související se službou možnosti příkazového řádku][MongoWindowsSvcOptions].

    toostart hello služba, spusťte tento příkaz:

        C:\> net start MongoDB
10. Teď, když MongoDB je nainstalován a spuštěn, je nutné tooopen port v bráně Windows Firewall, abyste mohli vzdáleně připojte tooMongoDB.  Z hello **spustit** nabídce vyberte možnost **nástroje pro správu** a potom **brány Windows Firewall s pokročilým zabezpečením**.
11. (a) v levém podokně hello vyberte **příchozí pravidla**.  V hello **akce** podokně na hello práva, vyberte **nové pravidlo...** .

    ![Brána Windows Firewall][Image1]

    b) v hello **pravidla Průvodce vytvořením nového příchozího**, vyberte **Port** a pak klikněte na **Další**.

    ![Brána Windows Firewall][Image2]

    c) vyberte **TCP** a potom **určité místní porty**.  Zadejte port "27017" (hello výchozí port MongoDB naslouchá na) a klikněte na tlačítko **Další**.

    ![Brána Windows Firewall][Image3]

    d) vyberte **povolit připojení hello** a klikněte na tlačítko **Další**.

    ![Brána Windows Firewall][Image4]

    e) klikněte na tlačítko **Další** znovu.

    ![Brána Windows Firewall][Image5]

    f) zadejte název pravidla hello, jako je například "MongoPort" a klikněte na tlačítko **Dokončit**.

    ![Brána Windows Firewall][Image6]

12. Pokud koncový bod nenakonfigurovali pro MongoDB při vytváření hello virtuálního počítače, můžete provést nyní. Je třeba pravidla brány firewall hello i hello koncový bod toobe možné tooconnect tooMongoDB vzdáleně.

  V hello portálu Azure, klikněte na **virtuálních počítačů (klasické)**, klikněte na tlačítko hello název nového virtuálního počítače a pak klikněte na tlačítko **koncové body**.

    ![Koncové body][Image7]

13. Klikněte na tlačítko **Přidat**.

14. Přidat koncový bod s názvem "Mongo", protokol **TCP**a obě **veřejné** a **privátní** porty sadu příliš "27017". Otevření tento port umožňuje přístup ke vzdálené toobe MongoDB.

    ![Koncové body][Image9]

> [!NOTE]
> Hello 27017 je hello výchozí port používaný MongoDB. Tento výchozí port můžete změnit zadáním hello `--port` parametr při spuštění serveru mongod.exe hello. Ujistěte se, že toogive hello stejné číslo portu v bráně firewall hello a hello "Mongo" koncového bodu v předchozích pokynů hello.
>
>

[MongoDownloads]: http://www.mongodb.org/downloads

[MongoWindowsSvcOptions]: http://www.mongodb.org/display/DOCS/Windows+Service


[Image1]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall1.png
[Image2]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall2.png
[Image3]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall3.png
[Image4]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall4.png
[Image5]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall5.png
[Image6]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall6.png
[Image7]: ./media/install-and-run-mongo-on-win2k8-vm/menusendpointadd.png
<!-- Removed 03/08/2017. Not in new portal. -->
<!-- [Image8]: ./media/install-and-run-mongo-on-win2k8-vm/WinVmAddEndpoint2.png
-->
[Image9]: ./media/install-and-run-mongo-on-win2k8-vm/newendpointdetails.png
