## <a name="what-is-queue-storage"></a>Co je služba Queue Storage?
Azure Queue storage je služba pro ukládání velkého počtu zpráv, které lze přistupovat z libovolné místo v hello, world prostřednictvím ověřených volání s využitím protokolu HTTP nebo HTTPS. Zpráva s jednou frontou může být až velikost too64 KB a jedna fronta můžete obsahovat miliony zpráv, až toohello limit celkové kapacity účtu úložiště.

Toto jsou některá běžná použití úložiště služby Queue Storage:

* Vytvoření backlogu práce tooprocess asynchronně
* Předávání zpráv z webové role tooan pracovního procesu Azure role Azure

## <a name="queue-service-concepts"></a>Koncepty služby front
Hello služba front obsahuje hello následující součásti:

![Fronta1](./media/storage-queue-concepts-include/queue1.png)

* **Formát adresy URL:** fronty jsou adresovatelné hello následující formát adresy URL:   
    http://`<storage account>`.queue.core.windows.net/`<queue>` 
  
    Následující adresa URL Hello řeší frontu v diagramu hello:  
  
    `http://myaccount.queue.core.windows.net/images-to-download`

* **Účet úložiště:** všechny přístup tooAzure úložiště se provádí prostřednictvím účtu úložiště. Podrobné informace o kapacitě účtu úložiště najdete v článku [Škálovatelnost a cíle výkonnosti služby Azure Storage](../articles/storage/common/storage-scalability-targets.md).
* **Fronta:** Fronta obsahuje sadu zpráv. Všechny zprávy musí být ve frontě. Všimněte si, že název fronty hello musí být psaný malými písmeny. Informace o pojmenování front najdete v tématu [Pojmenování front a metadata](https://msdn.microsoft.com/library/azure/dd179349.aspx).
* **Zpráva:** A zprávy, v libovolném formátu o až too64 KB. Hello maximální dobu, kterou může zpráva zůstat ve frontě hello je 7 dní.

