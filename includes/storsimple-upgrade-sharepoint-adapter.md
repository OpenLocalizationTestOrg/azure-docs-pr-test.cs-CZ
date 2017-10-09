<!--author=SharS last changed: 9/17/15-->

### <a name="upgrade-sharepoint-2010-toosharepoint-2013-and-then-install-hello-storsomple-adapter-for-sharepoint"></a>Upgradovat SharePoint 2010 tooSharePoint 2013 a potom nainstalovat hello StorSomple adaptéru pro službu SharePoint
> [!IMPORTANT]
> Všechny soubory, které byly dříve přesunout úložiště tooexternal prostřednictvím RBS nebudou k dispozici, dokud neskončí hello upgradu a hello RBS funkce je znovu povolena. uživatel toolimit vliv, proveďte všechny upgrade nebo přeinstalaci během plánované údržby.
> 
> 

#### <a name="tooupgrade-sharepoint-2010-toosharepoint-2013-and-then-install-hello-adapter"></a>tooSharePoint tooupgrade SharePoint 2010, 2013 a potom nainstalujte adaptér hello
1. Ve farmě služby SharePoint 2010 hello Poznámka hello BLOB Uložit cestu pro hello externalized objekty BLOB a hello databáze obsahu, pro které je RBS povolená. 
2. Nainstalujte a nakonfigurujte hello nové farmy serverů Sharepointu 2013. 
3. Přesun databáze, aplikace a kolekce webů z hello SharePoint 2010 farmy toohello nové farmy serverů Sharepointu 2013. Pokyny najdete příliš[přehled procesu upgradu tooSharePoint hello 2013](https://technet.microsoft.com/library/cc262483.aspx).
4. Nainstalujte hello adaptér StorSimple pro SharePoint v nové farmě hello. Přejděte příliš[hello instalaci adaptéru StorSimple pro službu SharePoint](#install-the-storsimple-adapter-for-sharepoint) postupy.
5. Pomocí informací o hello, kterou jste si poznamenali v kroku 1, povolte RBS hello stejná sada databází obsahu a zadejte hello stejný objekt BLOB Uložit cestu, která byla použita při instalaci hello SharePoint 2010. Přejděte příliš[konfigurace RBS](#configure-rbs) postupy. Po dokončení tohoto kroku, musí být přístupný z nové farmy hello dříve externalized soubory. 

### <a name="upgrade-hello-storsimple-adapter-for-sharepoint"></a>Upgrade hello adaptér StorSimple pro službu SharePoint
> [!IMPORTANT]
> Měli byste naplánovat tento upgrade toooccur během plánované údržby pro hello následujících důvodů:
> 
> * Externalized obsah dříve nebudete mít k dispozici, dokud bude znovu nainstalován adaptér hello.
> * Žádný obsah nahrát toohello lokality po odinstalaci hello předchozí verzi hello adaptér StorSimple pro službu SharePoint, ale před instalací nové verze hello, budou uloženy v databázi obsahu hello. Budete potřebovat toomove zařízení StorSimple obsahu toohello po instalaci hello nový adaptér. Můžete použít hello Microsoft` RBS Migrate()` rutiny prostředí PowerShell, které jsou součástí obsahu hello toomigrate služby SharePoint. Další informace najdete v tématu [migraci obsahu do nebo z RBS](https://technet.microsoft.com/library/ff628255.aspx). 
> 
> 

#### <a name="tooupgrade-hello-storsimple-adapter-for-sharepoint"></a>tooupgrade hello adaptér StorSimple pro službu SharePoint
1. Odinstalujte předchozí verze hello StorSimple adaptéru pro službu SharePoint.
   
   > [!NOTE]
   > Tato akce automaticky zakáže RBS v databázích obsahu hello. Existující objekty BLOB se však zůstane na zařízení StorSimple hello. Protože je zakázané RBS a objekty BLOB hello nebyly migrované back toohello databáze obsahu, všechny požadavky pro tyto objekty BLOB se nezdaří. 
   > 
   > 
2. Instalace hello nový adaptér StorSimple pro službu SharePoint. nový adaptér Hello hello databáze obsahu, které byly dříve povolit nebo zakázat pro RBS automaticky rozpozná a použije hello předchozí nastavení.

