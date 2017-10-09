<span data-ttu-id="c07d2-101">Hello [Microsoft Azure Configuration Manager Library pro .NET](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/) poskytuje třídu pro potřeby analýzy připojovacího řetězce z konfiguračního souboru.</span><span class="sxs-lookup"><span data-stu-id="c07d2-101">hello [Microsoft Azure Configuration Manager Library for .NET](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/) provides a class for parsing a connection string from a configuration file.</span></span> <span data-ttu-id="c07d2-102">Hello [CloudConfigurationManager](https://msdn.microsoft.com/library/azure/mt634650.aspx) třída analyzuje nastavení konfigurace bez ohledu na to, zda text hello klientská aplikace běží na ploše hello na mobilním zařízení, ve virtuálním počítači Azure nebo v cloudové službě Azure.</span><span class="sxs-lookup"><span data-stu-id="c07d2-102">hello [CloudConfigurationManager](https://msdn.microsoft.com/library/azure/mt634650.aspx) class parses configuration settings regardless of whether hello client application is running on hello desktop, on a mobile device, in an Azure virtual machine, or in an Azure cloud service.</span></span>

<span data-ttu-id="c07d2-103">tooreference hello balíček CloudConfigurationManager, přidejte následující hello `using` – direktiva:</span><span class="sxs-lookup"><span data-stu-id="c07d2-103">tooreference hello CloudConfigurationManager package, add hello following `using` directive:</span></span>

```csharp
using Microsoft.Azure; //Namespace for CloudConfigurationManager
```

<span data-ttu-id="c07d2-104">Tady je příklad, který ukazuje, jak tooretrieve připojovací řetězec z konfiguračního souboru:</span><span class="sxs-lookup"><span data-stu-id="c07d2-104">Here's an example that shows how tooretrieve a connection string from a configuration file:</span></span>

```csharp
// Parse hello connection string and return a reference toohello storage account.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));
```

<span data-ttu-id="c07d2-105">Použití hello Azure Configuration Manager není povinné.</span><span class="sxs-lookup"><span data-stu-id="c07d2-105">Using hello Azure Configuration Manager is optional.</span></span> <span data-ttu-id="c07d2-106">Můžete také použít API jako hello rozhraní .NET Framework [ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager.aspx) třídy.</span><span class="sxs-lookup"><span data-stu-id="c07d2-106">You can also use an API like hello .NET Framework's [ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager.aspx) class.</span></span>

