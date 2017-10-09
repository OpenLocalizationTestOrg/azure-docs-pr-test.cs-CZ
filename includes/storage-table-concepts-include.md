## <a name="what-is-table-storage"></a>Co je služba Table Storage
Služba Azure Table Storage ukládá velké objemy strukturovaných dat. Hello služby je úložištěm dat typu NoSQL, které přijímá ověřená volání z uvnitř i vně hello cloudu Azure. Tabulky Azure jsou ideální pro ukládání strukturovaných, nerelačních dat. Mezi běžná použití služby Table Storage patří:

* Ukládání terabajtů strukturovaných dat, která můžou obsluhovat škálované webové aplikace
* Ukládání datových sad, které nevyžadují komplexní spojení, cizí klíče nebo uložené postupy a které můžete denormalizovat kvůli rychlému přístupu
* Rychle dotazování na data pomocí clusterovaného indexu
* Přístup k datům pomocí hello protokolu OData a dotazů LINQ s knihovnami .NET datové služby WCF

Můžete použít tabulka úložiště toostore a dotazování obrovských sad strukturovaných, nerelačních dat a vaše tabulky se rostoucími požadavky škálovat.

## <a name="table-storage-concepts"></a>Koncepty služby Table Storage
Úložiště Table obsahuje hello následující součásti:

![Diagram komponent služby Table Storage][Table1]

* **Formát adresy URL:** Kód adresuje tabulky na účtu pomocí následujícího formátu adresy:   
  http://`<storage account>`.table.core.windows.net/`<table>`  
  
  Můžete vyřešit přímo pomocí této adresy s hello protokolu OData tabulek Azure. Další informace najdete na webu [OData.org][OData.org].
* **Účet úložiště:** všechny přístup tooAzure úložiště se provádí prostřednictvím účtu úložiště. Podrobné informace o kapacitě účtu úložiště najdete v článku [Škálovatelnost a cíle výkonnosti služby Azure Storage](../articles/storage/common/storage-scalability-targets.md).
* **Tabulka**: Tabulka je kolekcí entit. Tabulky nevynucují u entit schéma, což znamená, že jedna tabulka může obsahovat entity s různými sadami vlastností. Hello počet tabulek, které mohou obsahovat účet úložiště je omezena pouze limitem kapacity účtu úložiště hello.
* **Entity**: entita je sada vlastností, podobně jako řádek tooa databáze. Entita může mít až too1MB velikost.
* **Vlastnosti**: Vlastnost je pár název-hodnota. Každá entita může obsahovat data toostore too252 vlastnosti. Každá entita má také tři systémové vlastnosti, které určují klíč oddílu, klíč řádku a časové razítko. Entity s hello stejným klíčem oddílu můžete dotazovat rychleji a vkládat nebo aktualizovat v atomických operacích. Klíč řádku entity je jedinečným identifikátorem v rámci oddílu.

Podrobnosti o pojmenovávání tabulek a vlastnostech najdete v tématu [hello Principy datového modelu služby Table](/rest/api/storageservices/Understanding-the-Table-Service-Data-Model).

[Table1]: ./media/storage-table-concepts-include/table1.png
[OData.org]: http://www.odata.org/
