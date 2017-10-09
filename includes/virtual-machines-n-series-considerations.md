## <a name="deployment-considerations"></a>Aspekty nasazování

* Dostupnost virtuálních počítačů N-series, najdete v části [produkty podle oblasti](https://azure.microsoft.com/en-us/regions/services/).

* N-series virtuálních počítačů lze nasadit pouze v modelu nasazení Resource Manager hello.

* Při vytváření k N-series virtuálních počítačů pomocí portálu Azure, na hello hello **Základy** vyberte **typ disku virtuálního počítače** z **HDD**. velikost toochoose dostupné N-series na hello **velikost** okně klikněte na tlačítko **zobrazit všechny**.

* N-series virtuálních počítačů nepodporují disky virtuálních počítačů, které jsou zajišťované Azure Premium storage.

* Pokud chcete toodeploy víc než několik virtuálních počítačů N-series, zvažte průběžnými platbami předplatné nebo jiné možnosti nákupu. Pokud používáte [bezplatný účet Azure](https://azure.microsoft.com/free/), můžete použít pouze omezený počet výpočetních jader Azure.

* Může potřebovat tooincrease hello jader kvóty (podle oblasti) ve vašem předplatném Azure a zvýšit hello samostatné kvóty pro počet jader NC nebo vs. toorequest kvótu zvýšit, [otevřete žádosti o podporu online zákazníka](../articles/azure-supportability/how-to-create-azure-support-request.md) zdarma. Výchozí omezení se může lišit v závislosti na vaše předplatné kategorii.

* Jeden image virtuálního počítače můžete nasadit na virtuálních počítačích N-series je hello [virtuální počítač Azure datové vědy](../articles/machine-learning/machine-learning-data-science-virtual-machine-overview.md). Hello datové vědy virtuálního počítače provede předinstalaci a nakonfiguruje mnoha oblíbených datové vědy a přímý výukové nástroje. Provede také předinstalaci NVIDIA tesla – měrná GPU ovladače pro instance názvového kontextu.





