## <a name="create-a-simulated-device-app"></a><span data-ttu-id="1d160-101">Vytvoření aplikace simulovaného zařízení</span><span class="sxs-lookup"><span data-stu-id="1d160-101">Create a simulated device app</span></span>
<span data-ttu-id="1d160-102">V této části:</span><span class="sxs-lookup"><span data-stu-id="1d160-102">In this section, you:</span></span>

* <span data-ttu-id="1d160-103">Vytvořte konzolovou aplikaci Node.js, která odpovídá tooa přímá metoda volá hello cloudu</span><span class="sxs-lookup"><span data-stu-id="1d160-103">Create a Node.js console app that responds tooa direct method called by hello cloud</span></span>
* <span data-ttu-id="1d160-104">Aktivujete simulovanou aktualizaci firmwaru.</span><span class="sxs-lookup"><span data-stu-id="1d160-104">Trigger a simulated firmware update</span></span>
* <span data-ttu-id="1d160-105">Použití hello hlášené vlastnosti tooenable zařízení twin dotazy tooidentify zařízení a po poslední dokončení aktualizace firmwaru</span><span class="sxs-lookup"><span data-stu-id="1d160-105">Use hello reported properties tooenable device twin queries tooidentify devices and when they last completed a firmware update</span></span>

<span data-ttu-id="1d160-106">Krok 1: Vytvoření prázdnou složku s názvem **manageddevice**.</span><span class="sxs-lookup"><span data-stu-id="1d160-106">Step 1: Create an empty folder called **manageddevice**.</span></span>  <span data-ttu-id="1d160-107">V hello **manageddevice** složky, vytvořte soubor package.json pomocí následujícího příkazu na příkazovém řádku hello.</span><span class="sxs-lookup"><span data-stu-id="1d160-107">In hello **manageddevice** folder, create a package.json file using hello following command at your command prompt.</span></span> <span data-ttu-id="1d160-108">Přijměte všechny výchozí hodnoty hello:</span><span class="sxs-lookup"><span data-stu-id="1d160-108">Accept all hello defaults:</span></span>
   
    ```
    npm init
    ```

<span data-ttu-id="1d160-109">Krok 2: na příkazovém řádku v hello **manageddevice** složky, spusťte následující příkaz tooinstall hello hello **azure-iot-device** a **azure-iot zařízení mqtt** zařízení Balíčky sady SDK:</span><span class="sxs-lookup"><span data-stu-id="1d160-109">Step 2: At your command prompt in hello **manageddevice** folder, run hello following command tooinstall hello **azure-iot-device** and **azure-iot-device-mqtt** Device SDK packages:</span></span>
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

<span data-ttu-id="1d160-110">Krok 3: Pomocí textového editoru, vytvořte **dmpatterns_fwupdate_device.js** souboru v hello **manageddevice** složky.</span><span class="sxs-lookup"><span data-stu-id="1d160-110">Step 3: Using a text editor, create a **dmpatterns_fwupdate_device.js** file in hello **manageddevice** folder.</span></span>

<span data-ttu-id="1d160-111">Krok 4: Přidejte následující hello "vyžadovat" příkazy při spuštění hello hello **dmpatterns_fwupdate_device.js** souboru:</span><span class="sxs-lookup"><span data-stu-id="1d160-111">Step 4: Add hello following 'require' statements at hello start of hello **dmpatterns_fwupdate_device.js** file:</span></span>
   
    ```
    'use strict';
   
    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```
<span data-ttu-id="1d160-112">Krok 5: Přidejte **connectionString** proměnné a použít ho toocreate **klienta** instance.</span><span class="sxs-lookup"><span data-stu-id="1d160-112">Step 5: Add a **connectionString** variable and use it toocreate a **Client** instance.</span></span> <span data-ttu-id="1d160-113">Nahraďte hello `{yourdeviceconnectionstring}` zástupný symbol připojovacího řetězce hello dříve provedené poznamenejte si v části "Vytvoření identity zařízení" hello dříve:</span><span class="sxs-lookup"><span data-stu-id="1d160-113">Replace hello `{yourdeviceconnectionstring}` placeholder with hello connection string you previously made a note of in hello "Create a device identity" section previously:</span></span>
   
    ```
    var connectionString = '{yourdeviceconnectionstring}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

<span data-ttu-id="1d160-114">Krok 6: Přidejte hello následující funkce, která je použité tooupdate hlášené vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="1d160-114">Step 6: Add hello following function that is used tooupdate reported properties:</span></span>
   
    ```
    var reportFWUpdateThroughTwin = function(twin, firmwareUpdateValue) {
      var patch = {
          iothubDM : {
            firmwareUpdate : firmwareUpdateValue
          }
      };
   
      twin.properties.reported.update(patch, function(err) {
        if (err) throw err;
        console.log('twin state reported: ' + firmwareUpdateValue.status);
      });
    };
    ```

<span data-ttu-id="1d160-115">Krok 7: Přidejte následující funkce, které simulují stažení a použití bitové kopie firmware hello hello:</span><span class="sxs-lookup"><span data-stu-id="1d160-115">Step 7: Add hello following functions that simulate downloading and applying hello firmware image:</span></span>
   
    ```
    var simulateDownloadImage = function(imageUrl, callback) {
      var error = null;
      var image = "[fake image data]";
   
      console.log("Downloading image from " + imageUrl);
   
      callback(error, image);
    }
   
    var simulateApplyImage = function(imageData, callback) {
      var error = null;
   
      if (!imageData) {
        error = {message: 'Apply image failed because of missing image data.'};
      }
   
      callback(error);
    }
    ```

<span data-ttu-id="1d160-116">Krok 8: Přidejte následující funkce, že stav aktualizace firmwaru hello aktualizací prostřednictvím hello ohlásil vlastnosti příliš hello**čekání**.</span><span class="sxs-lookup"><span data-stu-id="1d160-116">Step 8: Add hello following function that updates hello firmware update status through hello reported properties too**waiting**.</span></span> <span data-ttu-id="1d160-117">Zařízení jsou obvykle zásad způsobuje hello zařízení toostart stažení a použití aktualizace hello informovány o k dispozici aktualizace a definovaného správcem.</span><span class="sxs-lookup"><span data-stu-id="1d160-117">Typically, devices are informed of an available update and an administrator defined policy causes hello device toostart downloading and applying hello update.</span></span> <span data-ttu-id="1d160-118">Tato funkce je kde hello tooenable logiku, která se má spustit zásady.</span><span class="sxs-lookup"><span data-stu-id="1d160-118">This function is where hello logic tooenable that policy should run.</span></span> <span data-ttu-id="1d160-119">Pro jednoduchost ukázka hello čeká čtyři sekund před pokračováním toodownload hello firmware obrázku:</span><span class="sxs-lookup"><span data-stu-id="1d160-119">For simplicity, hello sample waits for four seconds before proceeding toodownload hello firmware image:</span></span>
   
    ```
    var waitToDownload = function(twin, fwPackageUriVal, callback) {
      var now = new Date();
   
      reportFWUpdateThroughTwin(twin, {
        fwPackageUri: fwPackageUriVal,
        status: 'waiting',
        error : null,
        startedWaitingTime : now.toISOString()
      });
      setTimeout(callback, 4000);
    };
    ```

<span data-ttu-id="1d160-120">Krok 9: Přidejte následující funkce, že stav aktualizace firmwaru hello aktualizací prostřednictvím hello ohlásil vlastnosti příliš hello**stahování**.</span><span class="sxs-lookup"><span data-stu-id="1d160-120">Step 9: Add hello following function that updates hello firmware update status through hello reported properties too**downloading**.</span></span> <span data-ttu-id="1d160-121">Hello funkce pak simuluje stahování firmwaru a nakonec aktualizace hello tooeither stav aktualizace firmwaru **downloadFailed** nebo **downloadComplete**:</span><span class="sxs-lookup"><span data-stu-id="1d160-121">hello function then simulates a firmware download and finally updates hello firmware update status tooeither **downloadFailed** or **downloadComplete**:</span></span>
   
    ```
    var downloadImage = function(twin, fwPackageUriVal, callback) {
      var now = new Date();   
   
      reportFWUpdateThroughTwin(twin, {
        status: 'downloading',
      });
   
      setTimeout(function() {
        // Simulate download
        simulateDownloadImage(fwPackageUriVal, function(err, image) {
   
          if (err)
          {
            reportFWUpdateThroughTwin(twin, {
              status: 'downloadfailed',
              error: {
                code: error_code,
                message: error_message,
              }
            });
          }
          else {        
            reportFWUpdateThroughTwin(twin, {
              status: 'downloadComplete',
              downloadCompleteTime: now.toISOString(),
            });
   
            setTimeout(function() { callback(image); }, 4000);   
          }
        });
   
      }, 4000);
    }
    ```

<span data-ttu-id="1d160-122">Krok 10: Přidejte následující funkce, že stav aktualizace firmwaru hello aktualizací prostřednictvím hello ohlásil vlastnosti příliš hello**použití**.</span><span class="sxs-lookup"><span data-stu-id="1d160-122">Step 10: Add hello following function that updates hello firmware update status through hello reported properties too**applying**.</span></span> <span data-ttu-id="1d160-123">Hello funkce pak simuluje použití bitové kopie hello firmware a nakonec aktualizace hello tooeither stav aktualizace firmwaru **applyFailed** nebo **applyComplete**:</span><span class="sxs-lookup"><span data-stu-id="1d160-123">hello function then simulates applying hello firmware image and finally updates hello firmware update status tooeither **applyFailed** or **applyComplete**:</span></span>
    
    ```
    var applyImage = function(twin, imageData, callback) {
      var now = new Date();   
    
      reportFWUpdateThroughTwin(twin, {
        status: 'applying',
        startedApplyingImage : now.toISOString()
      });
    
      setTimeout(function() {
    
        // Simulate apply firmware image
        simulateApplyImage(imageData, function(err) {
          if (err) {
            reportFWUpdateThroughTwin(twin, {
              status: 'applyFailed',
              error: {
                code: err.error_code,
                message: err.error_message,
              }
            });
          } else { 
            reportFWUpdateThroughTwin(twin, {
              status: 'applyComplete',
              lastFirmwareUpdate: now.toISOString()
            });    
    
          }
        });
    
        setTimeout(callback, 4000);
    
      }, 4000);
    }
    ```

<span data-ttu-id="1d160-124">Krok 11: Přidejte následující hello funkce této obslužné rutiny hello **firmwareUpdate** přímá metoda a firmware více fáze hello zahájí proces aktualizovat:</span><span class="sxs-lookup"><span data-stu-id="1d160-124">Step 11: Add hello following function that handles hello **firmwareUpdate** direct method and initiates hello multi-stage firmware update process:</span></span>
    
    ```
    var onFirmwareUpdate = function(request, response) {
    
      // Respond hello cloud app for hello direct method
      response.send(200, 'FirmwareUpdate started', function(err) {
        if (!err) {
          console.error('An error occured when sending a method response:\n' + err.toString());
        } else {
          console.log('Response toomethod \'' + request.methodName + '\' sent successfully.');
        }
      });
    
      // Get hello parameter from hello body of hello method request
      var fwPackageUri = request.payload.fwPackageUri;
    
      // Obtain hello device twin
      client.getTwin(function(err, twin) {
        if (err) {
          console.error('Could not get device twin.');
        } else {
          console.log('Device twin acquired.');
    
          // Start hello multi-stage firmware update
          waitToDownload(twin, fwPackageUri, function() {
            downloadImage(twin, fwPackageUri, function(imageData) {
              applyImage(twin, imageData, function() {});    
            });  
          });
    
        }
      });
    }
    ```

<span data-ttu-id="1d160-125">Krok 12: Nakonec přidejte následující kód, který se připojuje tooyour IoT hub hello:</span><span class="sxs-lookup"><span data-stu-id="1d160-125">Step 12: Finally, add hello following code that connects tooyour IoT hub:</span></span>
    
    ```
    client.open(function(err) {
      if (err) {
        console.error('Could not connect tooIotHub client');
      }  else {
        console.log('Client connected tooIoT Hub.  Waiting for firmwareUpdate direct method.');
      }
    
      client.onDeviceMethod('firmwareUpdate', onFirmwareUpdate);
    });
    ```

> [!NOTE]
> <span data-ttu-id="1d160-126">věcí tookeep jednoduchý, tento kurz neimplementuje žádné zásady opakování.</span><span class="sxs-lookup"><span data-stu-id="1d160-126">tookeep things simple, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="1d160-127">V produkčním kódu, měli byste implementovat zásady opakování (například exponenciální zdvojnásobení) dle pokynů v článku na webu MSDN hello [přechodných chyb](https://msdn.microsoft.com/library/hh675232.aspx).</span><span class="sxs-lookup"><span data-stu-id="1d160-127">In production code, you should implement retry policies (such as an exponential backoff), as suggested in hello MSDN article [Transient Fault Handling](https://msdn.microsoft.com/library/hh675232.aspx).</span></span>
> 
> 