Jsou dva druhy účtů úložiště:

### <a name="general-purpose-storage-accounts"></a>Účty služby Storage pro obecné účely
Úložiště pro obecné účely poskytuje účet můžete získat přístup ke službám úložiště tooAzure například tabulky, fronty, soubory, objekty BLOB a Azure disky virtuálního počítače v rámci jednoho účtu. Tento typ účtu úložiště má dvě úrovně výkonu:

* Úroveň výkonu standardního úložiště, což vám umožní toostore tabulky, fronty, soubory, objekty BLOB a Azure disky virtuálního počítače.
* Úroveň výkonu prémiového úložiště, která aktuálně podporuje jenom disky virtuálních počítačů Azure. Podrobný popis služby Premium Storage najdete v článku [Úložiště Premium: vysoce výkonné úložiště pro úlohy virtuálních počítačů Azure](../articles/storage/common/storage-premium-storage.md).

### <a name="blob-storage-accounts"></a>Účty služby Blob Storage
Účet služby Blob Storage je specializovaný účet úložiště pro ukládání nestrukturovaných dat v podobě objektů blob do služby Azure Storage. Účty úložiště BLOB jsou podobné tooyour existující účty úložiště pro obecné účely a sdílet všechny hello skvělé odolnost, dostupnost, škálovatelnost a výkon funkce používají. jednotlivá včetně 100 % konzistentnost rozhraní API pro objekty BLOB bloku a doplňovací objekty BLOB. V případě aplikací, které vyžadují jenom úložiště objektů blob bloku nebo objektů blob doporučujeme používat účty úložiště objektů blob.

> [!NOTE]
> Účty úložiště Blob podporují pouze objekty blob bloku a doplňovací objekty blob, nepodporují objekty blob stránky.
> 
> 

Účty úložiště BLOB zpřístupňují hello **úroveň přístupu** atribut, který můžete zadat při vytváření účtů a později podle potřeby upravit. Existují dva typy úrovně přístupu, které můžete zadat na základě vzoru pro přístup k datům:

* A **horká** úroveň přístupu, která znamená, že bude hello objekty v účtu úložiště hello přistupovat častěji. To vám umožní toostore dat s nižšími náklady přístup.
* A **nástrojů** úroveň přístupu, která znamená, že hello objekty v účtu úložiště hello bude méně často přistupovat. To vám umožní toostore dat s nižšími náklady dat úložiště.

Pokud dojde ke změně v hello vzor používání vašich údajů, mohou také přepínat mezi úrovněmi přístupu kdykoli. Změna úrovně přístupu hello může mít za následek další poplatky. Další podrobnosti najdete v článku [Účty služby Blob Storage – ceny a fakturace](../articles/storage/blobs/storage-blob-storage-tiers.md#pricing-and-billing).

Další podrobnosti najdete v článku [Azure Blob Storage: úrovně Cool a Hot](../articles/storage/blobs/storage-blob-storage-tiers.md).

Před vytvořením účtu úložiště, musíte mít předplatné Azure, což je plán, který poskytuje přístup k různým službám Azure tooa. Azure můžete začít používat s [bezplatným účtem](https://azure.microsoft.com/pricing/free-trial/). Jakmile se rozhodnete toopurchase plán předplatného, můžete vybrat z různých [možností nákupu](https://azure.microsoft.com/pricing/purchase-options/). Pokud jste [předplatitelem MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), získáte bezplatné měsíční kredity, které můžete využít na služby Azure, včetně služby Azure Storage. Informace o cenách najdete v článku [Ceny služby Azure Storage](https://azure.microsoft.com/pricing/details/storage/).

jak toocreate účtu úložiště, najdete v části toolearn [vytvořit účet úložiště](../articles/storage/common/storage-create-storage-account.md#create-a-storage-account) další podrobnosti. Můžete vytvořit až too200 jednoznačně s názvem účty úložiště s jedním odběrem. Podrobné informace o omezeních účtu úložiště najdete v článku [Škálovatelnost a cíle výkonnosti služby Azure Storage](../articles/storage/common/storage-scalability-targets.md).

