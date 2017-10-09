Hello [Microsoft Azure Configuration Manager Library pro .NET](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/) poskytuje třídu pro potřeby analýzy připojovacího řetězce z konfiguračního souboru. Hello [CloudConfigurationManager](https://msdn.microsoft.com/library/azure/mt634650.aspx) třída analyzuje nastavení konfigurace bez ohledu na to, zda text hello klientská aplikace běží na ploše hello na mobilním zařízení, ve virtuálním počítači Azure nebo v cloudové službě Azure.

tooreference hello balíček CloudConfigurationManager, přidejte následující hello `using` – direktiva:

```csharp
using Microsoft.Azure; //Namespace for CloudConfigurationManager
```

Tady je příklad, který ukazuje, jak tooretrieve připojovací řetězec z konfiguračního souboru:

```csharp
// Parse hello connection string and return a reference toohello storage account.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));
```

Použití hello Azure Configuration Manager není povinné. Můžete také použít API jako hello rozhraní .NET Framework [ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager.aspx) třídy.

