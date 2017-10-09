### <a name="azure-storage-linked-service"></a>Propojená služba Azure Storage
Hello **propojená služba Azure Storage** vám umožní toolink služby úložiště Azure tooan účtu Azure data factory pomocí hello **klíč účtu**, který poskytuje objekt pro vytváření dat hello s globálním přístupem toohello Azure Úložiště. Hello následující tabulka obsahuje popis pro konkrétní tooAzure elementy JSON propojené služby úložiště.

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| type |vlastnost typu Hello musí být nastavena na: **azurestorage.** |Ano |
| připojovací řetězec |Zadejte informace potřebné pro vlastnost connectionString hello tooconnect tooAzure úložiště. |Ano |

V tématu hello následujícího článku pro klíč účtu hello tooview nebo zkopírování kroky pro Azure Storage: [zobrazení, kopírování a opětovné vytváření úložiště přístupové klíče](../articles/storage/common/storage-create-storage-account.md#manage-your-storage-account).

**Příklad:**  

```json
{  
    "name": "StorageLinkedService",  
    "properties": {  
        "type": "AzureStorage",  
        "typeProperties": {  
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"  
        }  
    }  
}  
```

### <a name="azure-storage-sas-linked-service"></a>Propojená služba úložiště Azure Sas
Sdílený přístupový podpis (SAS) poskytuje Delegovaný přístup tooresources ve vašem účtu úložiště. Je možné, že toogrant klienta omezené oprávnění tooobjects ve vašem účtu úložiště v zadaném časovém intervalu a zadanou sadu oprávnění, bez nutnosti tooshare klíče pro přístup k účtu. Hello SAS je identifikátor URI, který zahrnuje všechny hello informace potřebné pro ověřený přístup tooa úložiště prostředků v jeho parametry dotazu. tooaccess prostředky úložiště s hello SAS, hello klienta stačí toopass v odpovídající konstruktor toohello hello SAS nebo metoda. Podrobné informace o tokenu SAS naleznete v tématu [sdílené přístupové podpisy: vysvětlení hello modelu SAS](../articles/storage/common/storage-dotnet-shared-access-signature-part-1.md)

> [!IMPORTANT]
> Azure Data Factory podporuje nyní pouze **SAS služby** , ale není SAS účtu. V tématu [typy z sdílené přístupové podpisy](../articles/storage/common/storage-dotnet-shared-access-signature-part-1.md#types-of-shared-access-signatures) podrobnosti o těchto dvou typů a jak tooconstruct. Poznámka: hello SAS URL generable z portálu Azure nebo Storage Explorer je SAS účtu, který není podporován.
> 

Hello SAS úložiště Azure, propojené služby umožňuje toolink služby Azure data factory tooan účet úložiště Azure pomocí sdíleného přístupového podpisu (SAS). Poskytuje objekt pro vytváření dat hello přístup omezený nebo časově vázaných tooall nebo konkrétní prostředky (kontejner nebo objektů blob) v úložišti hello. Hello následující tabulka obsahuje popis pro konkrétní tooAzure elementy JSON úložiště SAS propojené služby. 

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| type |vlastnost typu Hello musí být nastavena na: **AzureStorageSas** |Ano |
| sasUri |Zadejte identifikátor URI podpis sdíleného přístupu toohello Azure Storage prostředky jako objekt blob, kontejneru nebo tabulky.  |Ano |

**Příklad:**

```json
{  
    "name": "StorageSasLinkedService",  
    "properties": {  
        "type": "AzureStorageSas",  
        "typeProperties": {  
            "sasUri": "<Specify SAS URI of hello Azure Storage resource>"   
        }  
    }  
}  
```

Při vytváření **identifikátor URI pro SAS**, vzhledem k tomu následující hello:  

* Nastavit vhodné pro čtení a zápis **oprávnění** u objektů na základě jak hello propojená služba (pro čtení, zápisu, čtení/zápis) se používá v datové továrně.
* Nastavit **čas vypršení platnosti** správně. Ujistěte se, že hello přístup tooAzure úložiště objektů v rámci hello aktivní období kanálu hello nevyprší platnost.
* Identifikátor URI by měl být vytvořen v hello správné kontejner nebo objekt blob nebo na úrovni tabulky podle hello potřebujete. Identifikátor Uri pro SAS tooan objektů blob v Azure umožňuje tooaccess služby Data Factory hello této konkrétní objekt blob. Kontejner objektů blob v Azure tooan identifikátor Uri pro SAS umožňuje tooiterate služby Data Factory hello prostřednictvím objektů BLOB v kontejneru. Pokud potřebujete přístup tooprovide více/méně objektů později, nebo aktualizace hello SAS URI, mějte na paměti, tooupdate hello propojené služby s hello nový identifikátor URI.   

