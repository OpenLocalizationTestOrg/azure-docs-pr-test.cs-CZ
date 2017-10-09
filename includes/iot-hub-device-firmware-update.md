## <a name="create-a-simulated-device-app"></a>Vytvoření aplikace simulovaného zařízení
V této části:

* Vytvořte konzolovou aplikaci Node.js, která odpovídá tooa přímá metoda volá hello cloudu
* Aktivujete simulovanou aktualizaci firmwaru.
* Použití hello hlášené vlastnosti tooenable zařízení twin dotazy tooidentify zařízení a po poslední dokončení aktualizace firmwaru

Krok 1: Vytvoření prázdnou složku s názvem **manageddevice**.  V hello **manageddevice** složky, vytvořte soubor package.json pomocí následujícího příkazu na příkazovém řádku hello. Přijměte všechny výchozí hodnoty hello:
   
    ```
    npm init
    ```

Krok 2: na příkazovém řádku v hello **manageddevice** složky, spusťte následující příkaz tooinstall hello hello **azure-iot-device** a **azure-iot zařízení mqtt** zařízení Balíčky sady SDK:
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

Krok 3: Pomocí textového editoru, vytvořte **dmpatterns_fwupdate_device.js** souboru v hello **manageddevice** složky.

Krok 4: Přidejte následující hello "vyžadovat" příkazy při spuštění hello hello **dmpatterns_fwupdate_device.js** souboru:
   
    ```
    'use strict';
   
    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```
Krok 5: Přidejte **connectionString** proměnné a použít ho toocreate **klienta** instance. Nahraďte hello `{yourdeviceconnectionstring}` zástupný symbol připojovacího řetězce hello dříve provedené poznamenejte si v části "Vytvoření identity zařízení" hello dříve:
   
    ```
    var connectionString = '{yourdeviceconnectionstring}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

Krok 6: Přidejte hello následující funkce, která je použité tooupdate hlášené vlastnosti:
   
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

Krok 7: Přidejte následující funkce, které simulují stažení a použití bitové kopie firmware hello hello:
   
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

Krok 8: Přidejte následující funkce, že stav aktualizace firmwaru hello aktualizací prostřednictvím hello ohlásil vlastnosti příliš hello**čekání**. Zařízení jsou obvykle zásad způsobuje hello zařízení toostart stažení a použití aktualizace hello informovány o k dispozici aktualizace a definovaného správcem. Tato funkce je kde hello tooenable logiku, která se má spustit zásady. Pro jednoduchost ukázka hello čeká čtyři sekund před pokračováním toodownload hello firmware obrázku:
   
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

Krok 9: Přidejte následující funkce, že stav aktualizace firmwaru hello aktualizací prostřednictvím hello ohlásil vlastnosti příliš hello**stahování**. Hello funkce pak simuluje stahování firmwaru a nakonec aktualizace hello tooeither stav aktualizace firmwaru **downloadFailed** nebo **downloadComplete**:
   
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

Krok 10: Přidejte následující funkce, že stav aktualizace firmwaru hello aktualizací prostřednictvím hello ohlásil vlastnosti příliš hello**použití**. Hello funkce pak simuluje použití bitové kopie hello firmware a nakonec aktualizace hello tooeither stav aktualizace firmwaru **applyFailed** nebo **applyComplete**:
    
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

Krok 11: Přidejte následující hello funkce této obslužné rutiny hello **firmwareUpdate** přímá metoda a firmware více fáze hello zahájí proces aktualizovat:
    
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

Krok 12: Nakonec přidejte následující kód, který se připojuje tooyour IoT hub hello:
    
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
> věcí tookeep jednoduchý, tento kurz neimplementuje žádné zásady opakování. V produkčním kódu, měli byste implementovat zásady opakování (například exponenciální zdvojnásobení) dle pokynů v článku na webu MSDN hello [přechodných chyb](https://msdn.microsoft.com/library/hh675232.aspx).
> 
> 