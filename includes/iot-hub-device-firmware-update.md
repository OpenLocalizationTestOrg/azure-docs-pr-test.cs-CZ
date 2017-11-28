## <a name="create-a-simulated-device-app"></a><span data-ttu-id="a482d-101">Vytvoření aplikace simulovaného zařízení</span><span class="sxs-lookup"><span data-stu-id="a482d-101">Create a simulated device app</span></span>
<span data-ttu-id="a482d-102">V této části:</span><span class="sxs-lookup"><span data-stu-id="a482d-102">In this section, you:</span></span>

* <span data-ttu-id="a482d-103">Vytvoříte konzolovou aplikaci Node.js, která bude reagovat na přímou metodu volanou cloudem.</span><span class="sxs-lookup"><span data-stu-id="a482d-103">Create a Node.js console app that responds to a direct method called by the cloud</span></span>
* <span data-ttu-id="a482d-104">Aktivujete simulovanou aktualizaci firmwaru.</span><span class="sxs-lookup"><span data-stu-id="a482d-104">Trigger a simulated firmware update</span></span>
* <span data-ttu-id="a482d-105">Pomocí ohlášených vlastností umožníte dotazům na dvojčata zařízení identifikovat zařízení a čas jejich poslední dokončené aktualizace firmwaru.</span><span class="sxs-lookup"><span data-stu-id="a482d-105">Use the reported properties to enable device twin queries to identify devices and when they last completed a firmware update</span></span>

<span data-ttu-id="a482d-106">Krok 1: Vytvoření prázdnou složku s názvem **manageddevice**.</span><span class="sxs-lookup"><span data-stu-id="a482d-106">Step 1: Create an empty folder called **manageddevice**.</span></span>  <span data-ttu-id="a482d-107">Ve složce **manageddevice** vytvořte soubor package.json pomocí následujícího příkazu na příkazovém řádku.</span><span class="sxs-lookup"><span data-stu-id="a482d-107">In the **manageddevice** folder, create a package.json file using the following command at your command prompt.</span></span> <span data-ttu-id="a482d-108">Přijměte všechny výchozí hodnoty:</span><span class="sxs-lookup"><span data-stu-id="a482d-108">Accept all the defaults:</span></span>
   
    ```
    npm init
    ```

<span data-ttu-id="a482d-109">Krok 2: na příkazovém řádku v **manageddevice** složky, spusťte následující příkaz k instalaci **azure-iot-device** a **azure-iot zařízení mqtt** zařízení SDK balíčky:</span><span class="sxs-lookup"><span data-stu-id="a482d-109">Step 2: At your command prompt in the **manageddevice** folder, run the following command to install the **azure-iot-device** and **azure-iot-device-mqtt** Device SDK packages:</span></span>
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

<span data-ttu-id="a482d-110">Krok 3: Pomocí textového editoru, vytvořte **dmpatterns_fwupdate_device.js** v soubor **manageddevice** složky.</span><span class="sxs-lookup"><span data-stu-id="a482d-110">Step 3: Using a text editor, create a **dmpatterns_fwupdate_device.js** file in the **manageddevice** folder.</span></span>

<span data-ttu-id="a482d-111">Krok 4: Přidejte následující příkazy na začátku "vyžadovat" **dmpatterns_fwupdate_device.js** souboru:</span><span class="sxs-lookup"><span data-stu-id="a482d-111">Step 4: Add the following 'require' statements at the start of the **dmpatterns_fwupdate_device.js** file:</span></span>
   
    ```
    'use strict';
   
    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```
<span data-ttu-id="a482d-112">Krok 5: Přidejte **connectionString** proměnné a použít ho k vytvoření **klienta** instance.</span><span class="sxs-lookup"><span data-stu-id="a482d-112">Step 5: Add a **connectionString** variable and use it to create a **Client** instance.</span></span> <span data-ttu-id="a482d-113">Nahraďte zástupný text `{yourdeviceconnectionstring}` připojovacím řetězcem, který jste si zaznamenali dříve v části Vytvoření identity zařízení:</span><span class="sxs-lookup"><span data-stu-id="a482d-113">Replace the `{yourdeviceconnectionstring}` placeholder with the connection string you previously made a note of in the "Create a device identity" section previously:</span></span>
   
    ```
    var connectionString = '{yourdeviceconnectionstring}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

<span data-ttu-id="a482d-114">Krok 6: Přidejte následující funkce, které se používá k aktualizaci hlášené vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="a482d-114">Step 6: Add the following function that is used to update reported properties:</span></span>
   
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

<span data-ttu-id="a482d-115">Krok 7: Přidejte následující funkce, které simulují stažením a použitím firmware bitové kopie:</span><span class="sxs-lookup"><span data-stu-id="a482d-115">Step 7: Add the following functions that simulate downloading and applying the firmware image:</span></span>
   
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

<span data-ttu-id="a482d-116">Krok 8: Přidejte následující funkce, která aktualizuje stav aktualizace firmwaru je hlášen vlastnostech k **čekání**.</span><span class="sxs-lookup"><span data-stu-id="a482d-116">Step 8: Add the following function that updates the firmware update status through the reported properties to **waiting**.</span></span> <span data-ttu-id="a482d-117">Zařízení jsou obvykle informována o dostupné aktualizaci a správcem definovaná zásada způsobí, že začnou stahovat a aplikovat aktualizaci firmwaru.</span><span class="sxs-lookup"><span data-stu-id="a482d-117">Typically, devices are informed of an available update and an administrator defined policy causes the device to start downloading and applying the update.</span></span> <span data-ttu-id="a482d-118">V této funkci by měla běžet logika, která povoluje tuto zásadu.</span><span class="sxs-lookup"><span data-stu-id="a482d-118">This function is where the logic to enable that policy should run.</span></span> <span data-ttu-id="a482d-119">Pro jednoduchost ukázky čeká čtyři sekund, než budete pokračovat stáhnout do firmwaru:</span><span class="sxs-lookup"><span data-stu-id="a482d-119">For simplicity, the sample waits for four seconds before proceeding to download the firmware image:</span></span>
   
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

<span data-ttu-id="a482d-120">Krok 9: Přidejte následující funkce, která aktualizuje stav aktualizace firmwaru je hlášen vlastnostech k **stahování**.</span><span class="sxs-lookup"><span data-stu-id="a482d-120">Step 9: Add the following function that updates the firmware update status through the reported properties to **downloading**.</span></span> <span data-ttu-id="a482d-121">Funkce pak simuluje stahování firmwaru a nakonec aktualizuje stav aktualizace firmwaru buď na **downloadFailed** (Stahování selhalo), nebo **downloadComplete** (Stahování dokončeno):</span><span class="sxs-lookup"><span data-stu-id="a482d-121">The function then simulates a firmware download and finally updates the firmware update status to either **downloadFailed** or **downloadComplete**:</span></span>
   
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

<span data-ttu-id="a482d-122">Krok 10: Přidejte následující funkce, která aktualizuje stav aktualizace firmwaru je hlášen vlastnostech k **použití**.</span><span class="sxs-lookup"><span data-stu-id="a482d-122">Step 10: Add the following function that updates the firmware update status through the reported properties to **applying**.</span></span> <span data-ttu-id="a482d-123">Funkce pak simuluje aplikování image firmwaru a nakonec aktualizuje stav aktualizace firmwaru buď na **applyFailed** (Aplikování selhalo), nebo **applyComplete** (Aplikování dokončeno):</span><span class="sxs-lookup"><span data-stu-id="a482d-123">The function then simulates applying the firmware image and finally updates the firmware update status to either **applyFailed** or **applyComplete**:</span></span>
    
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

<span data-ttu-id="a482d-124">Krok 11: Přidejte následující funkce, která zpracovává **firmwareUpdate** přímá metoda a zahájí aktualizaci firmwaru více fáze procesu:</span><span class="sxs-lookup"><span data-stu-id="a482d-124">Step 11: Add the following function that handles the **firmwareUpdate** direct method and initiates the multi-stage firmware update process:</span></span>
    
    ```
    var onFirmwareUpdate = function(request, response) {
    
      // Respond the cloud app for the direct method
      response.send(200, 'FirmwareUpdate started', function(err) {
        if (!err) {
          console.error('An error occured when sending a method response:\n' + err.toString());
        } else {
          console.log('Response to method \'' + request.methodName + '\' sent successfully.');
        }
      });
    
      // Get the parameter from the body of the method request
      var fwPackageUri = request.payload.fwPackageUri;
    
      // Obtain the device twin
      client.getTwin(function(err, twin) {
        if (err) {
          console.error('Could not get device twin.');
        } else {
          console.log('Device twin acquired.');
    
          // Start the multi-stage firmware update
          waitToDownload(twin, fwPackageUri, function() {
            downloadImage(twin, fwPackageUri, function(imageData) {
              applyImage(twin, imageData, function() {});    
            });  
          });
    
        }
      });
    }
    ```

<span data-ttu-id="a482d-125">Krok 12: Nakonec přidejte následující kód, který se připojuje ke službě IoT hub:</span><span class="sxs-lookup"><span data-stu-id="a482d-125">Step 12: Finally, add the following code that connects to your IoT hub:</span></span>
    
    ```
    client.open(function(err) {
      if (err) {
        console.error('Could not connect to IotHub client');
      }  else {
        console.log('Client connected to IoT Hub.  Waiting for firmwareUpdate direct method.');
      }
    
      client.onDeviceMethod('firmwareUpdate', onFirmwareUpdate);
    });
    ```

> [!NOTE]
> <span data-ttu-id="a482d-126">Za účelem zjednodušení tento kurz neimplementuje žádné zásady opakování.</span><span class="sxs-lookup"><span data-stu-id="a482d-126">To keep things simple, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="a482d-127">V produkčním kódu, měli byste implementovat zásady opakování (například exponenciální zdvojnásobení) dle pokynů v článku na webu MSDN [přechodných chyb](https://msdn.microsoft.com/library/hh675232.aspx).</span><span class="sxs-lookup"><span data-stu-id="a482d-127">In production code, you should implement retry policies (such as an exponential backoff), as suggested in the MSDN article [Transient Fault Handling](https://msdn.microsoft.com/library/hh675232.aspx).</span></span>
> 
> 