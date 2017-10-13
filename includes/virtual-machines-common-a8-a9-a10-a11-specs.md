

## <a name="deployment-considerations"></a>Aspekty nasazování
* **Předplatné Azure** – jak nasadit víc než několik instancí náročné, zvažte průběžnými platbami předplatné nebo jiné možnosti nákupu. Pokud používáte [bezplatný účet Azure](https://azure.microsoft.com/free/), můžete použít pouze omezený počet výpočetních jader Azure.

* **Ceny a dostupnosti** -velikosti tyto virtuálních počítačů jsou nabízena pouze v standardní cenovou úroveň. Zkontrolujte [produkty dostupné podle oblasti] (https://azure.microsoft.com/regions/services/) pro dostupnost v oblastech Azure. 
* **Kvóta jader** – možná budete muset zvýšit kvótu jader v předplatném Azure z výchozí hodnoty. Vaše předplatné může také omezit počet jader, který můžete nasadit v určité rodiny velikost virtuálního počítače, včetně H-series. Požádat o zvýšení kvóty, [otevřete žádosti o podporu online zákazníka](../articles/azure-supportability/how-to-create-azure-support-request.md) zdarma. (Výchozí omezení se liší v závislosti na vaše předplatné kategorie).
  
  > [!NOTE]
  > Pokud máte požadavků na kapacitu ve velkém měřítku, obraťte se na podporu Azure. Kvótách Azure jsou platební limity, není kapacity záruky. Bez ohledu na vaší kvóty vám jsou pouze účtovat jader použít.
  > 
  > 
* **Virtuální síť** – na Azure [virtuální sítě](https://azure.microsoft.com/documentation/services/virtual-network/) není potřeba použít instance náročné. Však v mnoha nasazeních musíte mít aspoň cloudové virtuální síť Azure nebo připojení site-to-site Pokud budete potřebovat pro přístup k místním prostředkům. V případě potřeby vytvořte nové virtuální sítě pro nasazení instance. Přidávání virtuálních počítačů náročné do virtuální sítě ve skupině vztahů není podporováno.
* **Změna velikosti** – kvůli speciální hardware, můžete pouze změnit velikost náročné instancí v rámci stejné rodiny velikost (H-series nebo náročné A-series). Například můžete nastavit velikost pouze H-series virtuální počítač z jednoho velikost řady H do jiného. Kromě toho není podporována Změna velikosti z výpočetní náročné velikost na velikost náročné.  
