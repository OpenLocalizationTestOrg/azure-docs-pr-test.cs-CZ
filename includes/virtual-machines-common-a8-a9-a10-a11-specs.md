

## <a name="deployment-considerations"></a>Aspekty nasazování
* **Předplatné Azure** – toodeploy víc než několik instancí náročné, vezměte v úvahu průběžnými platbami předplatné nebo jiné možnosti nákupu. Pokud používáte [bezplatný účet Azure](https://azure.microsoft.com/free/), můžete použít pouze omezený počet výpočetních jader Azure.

* **Ceny a dostupnosti** -velikosti tyto virtuálních počítačů jsou nabízena pouze v hello standardní cenovou úroveň. Zkontrolujte [produkty dostupné podle oblasti] (https://azure.microsoft.com/regions/services/) pro dostupnost v oblastech Azure. 
* **Kvóta jader** – může být nutné tooincrease hello jader kvóty ve vašem předplatném Azure z hello výchozí hodnotu. Vaše předplatné může také omezit hello počet jader, který můžete nasadit v určité rodiny velikost virtuálního počítače, včetně hello H-series. toorequest kvótu zvýšit, [otevřete žádosti o podporu online zákazníka](../articles/azure-supportability/how-to-create-azure-support-request.md) zdarma. (Výchozí omezení se liší v závislosti na vaše předplatné kategorie).
  
  > [!NOTE]
  > Pokud máte požadavků na kapacitu ve velkém měřítku, obraťte se na podporu Azure. Kvótách Azure jsou platební limity, není kapacity záruky. Bez ohledu na vaší kvóty vám jsou pouze účtovat jader použít.
  > 
  > 
* **Virtuální síť** – na Azure [virtuální sítě](https://azure.microsoft.com/documentation/services/virtual-network/) není požadováno toouse hello náročné instancí. Ale v mnoha nasazeních musíte mít aspoň cloudové virtuální síť Azure, nebo připojení site-to-site, pokud potřebujete tooaccess místních prostředků. V případě potřeby vytvořte novou virtuální síť toodeploy hello instancí. Přidání virtuální sítě tooa náročné virtuální počítače ve skupině vztahů není podporováno.
* **Změna velikosti** – kvůli speciální hardware, můžete pouze změnit velikost náročné instancí v rámci hello stejná velikost řady (H-series nebo náročné A-series). Například můžete nastavit velikost pouze H-series virtuální počítač z jednoho tooanother velikost H-series. Kromě toho není podporována Změna velikosti z s velikostí, velikost výpočetních náročné tooa náročné.  
