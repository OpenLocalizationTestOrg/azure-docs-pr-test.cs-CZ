## <a name="create-a-simulated-device-app"></a>Vytvoření aplikace simulovaného zařízení
V této části:

* Vytvoříte konzolovou aplikaci Node.js, která bude reagovat na přímou metodu volanou cloudem.
* Aktivujete simulovanou aktualizaci firmwaru.
* Pomocí ohlášených vlastností umožníte dotazům na dvojčata zařízení identifikovat zařízení a čas jejich poslední dokončené aktualizace firmwaru.

Krok 1: Vytvoření prázdnou složku s názvem **manageddevice**.  Ve složce **manageddevice** vytvořte soubor package.json pomocí následujícího příkazu na příkazovém řádku. Přijměte všechny výchozí hodnoty:
   
    ```
    npm init
    ```

Krok 2: na příkazovém řádku v **manageddevice** složky, spusťte následující příkaz k instalaci **azure-iot-device** a **azure-iot zařízení mqtt** zařízení SDK balíčky:
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

Krok 3: Pomocí textového editoru, vytvořte **dmpatterns_fwupdate_device.js** v soubor **manageddevice** složky.

Krok 4: Přidejte následující příkazy na začátku "vyžadovat" **dmpatterns_fwupdate_device.js** souboru:
   
    ```
    'use strict';
   
    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```
Krok 5: Přidejte **connectionString** proměnné a použít ho k vytvoření **klienta** instance. Nahraďte zástupný text `{yourdeviceconnectionstring}` připojovacím řetězcem, který jste si zaznamenali dříve v části Vytvoření identity zařízení:
   
    ```
    var connectionString = '{yourdeviceconnectionstring}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

Krok 6: Přidejte následující funkce, které se používá k aktualizaci hlášené vlastnosti:
   
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

Krok 7: Přidejte následující funkce, které simulují stažením a použitím firmware bitové kopie:
   
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

Krok 8: Přidejte následující funkce, která aktualizuje stav aktualizace firmwaru je hlášen vlastnostech k **čekání**. Zařízení jsou obvykle informována o dostupné aktualizaci a správcem definovaná zásada způsobí, že začnou stahovat a aplikovat aktualizaci firmwaru. V této funkci by měla běžet logika, která povoluje tuto zásadu. Pro jednoduchost ukázky čeká čtyři sekund, než budete pokračovat stáhnout do firmwaru:
   
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

Krok 9: Přidejte následující funkce, která aktualizuje stav aktualizace firmwaru je hlášen vlastnostech k **stahování**. Funkce pak simuluje stahování firmwaru a nakonec aktualizuje stav aktualizace firmwaru buď na **downloadFailed** (Stahování selhalo), nebo **downloadComplete** (Stahování dokončeno):
   
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

Krok 10: Přidejte následující funkce, která aktualizuje stav aktualizace firmwaru je hlášen vlastnostech k **použití**. Funkce pak simuluje aplikování image firmwaru a nakonec aktualizuje stav aktualizace firmwaru buď na **applyFailed** (Aplikování selhalo), nebo **applyComplete** (Aplikování dokončeno):
    
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

Krok 11: Přidejte následující funkce, která zpracovává **firmwareUpdate** přímá metoda a zahájí aktualizaci firmwaru více fáze procesu:
    
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

Krok 12: Nakonec přidejte následující kód, který se připojuje ke službě IoT hub:
    
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
> Za účelem zjednodušení tento kurz neimplementuje žádné zásady opakování. V produkčním kódu, měli byste implementovat zásady opakování (například exponenciální zdvojnásobení) dle pokynů v článku na webu MSDN [přechodných chyb](https://msdn.microsoft.com/library/hh675232.aspx).
> 
> 