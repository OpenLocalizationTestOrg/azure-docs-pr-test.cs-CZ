
Diagnostika problémů s cloudové služby Microsoft Azure vyžaduje shromažďování souborů protokolů služby hello na virtuální počítače jsou prováděny hello problémy. Můžete použít hello AzureLogCollector rozšíření na vyžádání tooperfom jednorázové shromažďování protokolů z jedné nebo více virtuálních počítačů služby cloudu (z webových rolí a rolí pracovního procesu) a přenos hello shromážděné soubory tooan účtu úložiště Azure – všechny bez vzdálené přihlášení tooany hello virtuálních počítačů.

> [!NOTE]
> Popisy pro většinu hello protokolovat informace lze najít na http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.asp.
> 
> 

Existují dva režimy kolekce závisí na typech hello toobe soubory shromážděny.

* Azure hostovaného agenta protokoly pouze (GA). Tento režim kolekce zahrnuje všechny agenty hostů související tooAzure hello protokoly a dalšími součástmi Azure.
* Všechny protokoly (úplná záloha). Tento režim kolekce bude shromažďovat všechny soubory v režimu GA plus:
  
  * protokoly událostí systému a aplikací
  * Protokoly chyb HTTP
  * Protokoly služby IIS
  * Protokoly instalace
  * Další systémové protokoly

V obou režimech kolekce složky kolekcí další data lze pomocí kolekce hello strukturu:

* **Název**: název hello hello kolekce, která se použije jako hello název podsložky uvnitř toobe soubor zip hello shromažďují.
* **Umístění**: hello cesta toohello složky na virtuálním počítači hello kde budou shromažďovány souboru.
* **SearchPattern**: hello vzor názvů hello toobe soubory shromážděny. Výchozí hodnota je "*"
* **Rekurzivní**: Pokud budou soubory hello shromážděných rekurzivně složce hello.

## <a name="prerequisites"></a>Požadavky
* Potřebujete toohave účet úložiště pro soubory zip toosave generované rozšíření.
* Ujistěte se, že používáte V0.8.0 rutiny Azure PowerShell nebo vyšší. Další informace najdete v tématu [Azure stáhne](https://azure.microsoft.com/downloads/).

## <a name="add-hello-extension"></a>Přidat rozšíření hello
Můžete použít [Microsoft Azure PowerShell](https://msdn.microsoft.com/library/dn495240.aspx) rutiny nebo [rozhraní API REST pro správu služby](https://msdn.microsoft.com/library/ee460799.aspx) tooadd hello AzureLogCollector rozšíření.

Pro cloudové služby, hello existující rutiny Azure Powershellu, **Set-AzureServiceExtension**, může být použité tooenable hello rozšíření u instancí role cloudové služby. Pokaždé, když je toto rozšíření povolené prostřednictvím této rutiny, aktivuje se u instancí role hello vybrané vybraných rolí shromáždění protokolů.

Pro virtuální počítače, hello existující rutiny Azure Powershellu, **Set-AzureVMExtension**, může být rozšíření hello tooenable používané virtuálními počítači. Pokaždé, když je toto rozšíření povolené pomocí rutin hello, aktivuje se na každou instanci shromáždění protokolů.

Toto rozšíření interně používá hello na základě JSON PublicConfiguration a PrivateConfiguration. Hello následuje hello rozložení ukázkové JSON pro veřejné a privátní konfigurace.

### <a name="publicconfiguration"></a>PublicConfiguration
    {
        "Instances":  "*",
        "Mode":  "Full",
        "SasUri":  "SasUri tooyour storage account with sp=wl",
        "AdditionalData":
        [
          {
                  "Name":  "StorageData",
                  "Location":  "%roleroot%storage",
                  "SearchPattern":  "*.*",
                  "Recursive":  "true"
          },
          {
                "Name":  "CustomDataFolder2",
                "Location":  "c:\customFolder",
                "SearchPattern":  "*.log",
                "Recursive":  "false"
          },
        ]
    }

### <a name="privateconfiguration"></a>PrivateConfiguration
    {

    }

> [!NOTE]
> Toto rozšíření nevyžaduje **privateConfiguration**. Můžete jenom zadat prázdnou strukturou hello **– PrivateConfiguration** argument.
> 
> 

Proveďte jeden z hello dvě následující kroky tooadd hello AzureLogCollector tooone nebo více instancí služeb Cloud nebo virtuální počítač z vybrané role, které aktivuje hello kolekce na každý počítač toorun a odesílat hello shromážděné soubory tooAzure účtu zadat.

## <a name="adding-as-a-service-extension"></a>Přidání jako rozšíření služby
1. Postupujte podle hello pokyny tooconnect prostředí Azure PowerShell tooyour předplatné.
2. Zadejte název služby hello, slotu, role a role instance toowhich chcete tooadd a povolit rozšíření AzureLogCollector hello.
   
        #Specify your cloud service name
        $ServiceName = 'extensiontest2'
   
        #Specify hello slot. 'Production' or 'Staging'
        $slot = 'Production'
   
        #Specified hello roles on which hello extension will be installed and enabled
        $roles = @("WorkerRole1","WebRole1")
   
        #Specify hello instances on which extension will be installed and enabled.  Use wildcard * for all instances
        $instances = @("*")
   
        #Specify hello collection mode, "Full" or "GA"
        $mode = "GA"
3. Zadejte složku hello další data, pro které se budou shromažďovat soubory (Tento krok je volitelný).
   
        #add one location
        $a1 = New-Object PSObject
   
        $a1 | Add-Member -MemberType NoteProperty -Name "Name" -Value "StorageData"
        $a1 | Add-Member -MemberType NoteProperty -Name "SearchPattern" -Value "*"
        $a1 | Add-Member -MemberType NoteProperty -Name "Location" -Value "%roleroot%storage"  #%roleroot% is normally E: or F: drive
        $a1 | Add-Member -MemberType NoteProperty -Name "Recursive" -Value "true"
   
        $AdditionalDataList+= $a1
              #more locations can be added....
   
   > [!NOTE]
   > Můžete použít token `%roleroot%` toospecify hello role kořenové jednotce vzhledem k tomu nepoužívá pevnou jednotku.
   > 
   > 
4. Zadejte název účtu úložiště Azure hello a klíče toowhich shromážděné soubory budou odeslány.
   
        $StorageAccountName = 'YourStorageAccountName'
        $StorageAccountKey  = ‘YouStorageAccountKey'
5. Volání hello SetAzureServiceLogCollector.ps1 (zahrnutá na konci hello tohoto článku hello) jako způsobem tooenable hello AzureLogCollector rozšíření pro cloudové služby. Po dokončení provádění hello hello nahrát soubor naleznete v části`https://YouareStorageAccountName.blob.core.windows.net/vmlogs`
   
        .\SetAzureServiceLogCollector.ps1 -ServiceName YourCloudServiceName  -Roles $roles  -Instances $instances –Mode $mode -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey -AdditionDataLocationList $AdditionalDataList

Hello následuje hello Definice hello parametry předané toohello skriptu. (To je zkopírovat níže také.)

    [CmdletBinding(SupportsShouldProcess = $true)]

    param (
      [Parameter(Mandatory=$true)]
    [string]   $ServiceName,

    [Parameter(Mandatory=$false)]
    [string[]] $Roles ,

    [Parameter(Mandatory=$false)]
    [string[]] $Instances,

    [Parameter(Mandatory=$false)]
    [string]   $Slot = 'Production',

    [Parameter(Mandatory=$false)]
    [string]   $Mode = 'Full',

    [Parameter(Mandatory=$false)]
    [string]   $StorageAccountName,

    [Parameter(Mandatory=$false)]
    [string]   $StorageAccountKey,

    [Parameter(Mandatory=$false)]
    [PSObject[]] $AdditionDataLocationList = $null
    )

* *ServiceName*: název vaší cloudové služby.
* *Role*: seznam rolí, například "WebRole1" nebo "WorkerRole1".
* *Instance*: seznam názvů hello instancí role oddělené čárkou – použijte hello zástupný řetězec ("*") pro všechny instance role.
* *Slot*: název slotu. "Výroba" nebo "Přípravy".
* *Režim*: režim kolekce. "Úplná" nebo "GA".
* *StorageAccountName*: název Azure účet úložiště pro ukládání shromážděných dat.
* *StorageAccountKey*: název Azure klíč účtu úložiště.
* *AdditionalDataLocationList*: seznam hello strukturu:
  
      {
      String Name,
      String Location,
      String SearchPattern,
      Bool   Recursive
      }

## <a name="adding-as-a-vm-extension"></a>Přidání jako rozšíření virtuálního počítače
Postupujte podle hello pokyny tooconnect prostředí Azure PowerShell tooyour předplatné.

1. Zadejte název služby hello, virtuálních počítačů a režim kolekce hello.
   
        #Specify your cloud service name
        $ServiceName = 'YourCloudServiceName'
   
        #Specify hello VM name
        $VMName = "'YourVMName'"
   
        #Specify hello collection mode, "Full" or "GA"
        $mode = "GA"
   
        Specify hello additional data folder for which files will be collected (this step is optional).
   
        #add one location
        $a1 = New-Object PSObject
   
        $a1 | Add-Member -MemberType NoteProperty -Name "Name" -Value "StorageData"
        $a1 | Add-Member -MemberType NoteProperty -Name "SearchPattern" -Value "*"
        $a1 | Add-Member -MemberType NoteProperty -Name "Location" -Value "%roleroot%storage"  #%roleroot% is normally E: or F: drive
        $a1 | Add-Member -MemberType NoteProperty -Name "Recursive" -Value "true"
   
        $AdditionalDataList+= $a1
              #more locations can be added....
2. Zadejte název účtu úložiště Azure hello a klíče toowhich shromážděné soubory budou odeslány.
   
        $StorageAccountName = 'YourStorageAccountName'
        $StorageAccountKey  = ‘YouStorageAccountKey'
3. Volání hello SetAzureVMLogCollector.ps1 (zahrnutá na konci hello tohoto článku hello) jako způsobem tooenable hello AzureLogCollector rozšíření pro cloudové služby. Po dokončení provádění hello můžete najít v souboru hello nahrán pod https://YouareStorageAccountName.blob.core.windows.net/vmlogs

Hello následuje hello Definice hello parametry předané toohello skriptu. (To je zkopírovat níže také.)

    [CmdletBinding(SupportsShouldProcess = $true)]

    param (
        [Parameter(Mandatory=$true)]
      [string]   $ServiceName,

      [Parameter(Mandatory=$false)]
      [string] $VMName ,

        [Parameter(Mandatory=$false)]
      [string]   $Mode = 'Full',

      [Parameter(Mandatory=$false)]
      [string]   $StorageAccountName,

      [Parameter(Mandatory=$false)]
      [string]   $StorageAccountKey,

      [Parameter(Mandatory=$false)]
      [PSObject[]] $AdditionDataLocationList = $null
      )

* Název služby: Váš název cloudové služby.
* Název hello VMName hello virtuálních počítačů.
* Režim: Režim kolekce. "Úplná" nebo "GA".
* StorageAccountName: Název účtu úložiště Azure pro ukládání shromážděna data.
* StorageAccountKey: Název klíč účtu úložiště Azure.
* AdditionalDataLocationList: Seznam hello strukturu:

```
      {
        String Name,
        String Location,
        String SearchPattern,
        Bool   Recursive
      }
```

## <a name="extention-powershell-script-files"></a>Soubory skriptu prostředí PowerShell vytvořit vícejazyčný
SetAzureServiceLogCollector.ps1

    [CmdletBinding(SupportsShouldProcess = $true)]

    param (
                  [Parameter(Mandatory=$true)]
                  [string]   $ServiceName,

                  [Parameter(Mandatory=$false)]
                  [string[]] $Roles ,

                  [Parameter(Mandatory=$false)]
                  [string[]] $Instances = '*',

                  [Parameter(Mandatory=$false)]
                  [string]   $Slot = 'Production',

                  [Parameter(Mandatory=$false)]
                  [string]   $Mode = 'Full',

                  [Parameter(Mandatory=$false)]
                  [string]   $StorageAccountName,

                  [Parameter(Mandatory=$false)]
                  [string]   $StorageAccountKey,

                  [Parameter(Mandatory=$false)]
                  [PSObject[]] $AdditionDataLocationList = $null
            )

    $publicConfig = New-Object PSObject

    if ($Instances -ne $null -and $Instances.Count -gt 0)  #Instances should be seperated by ,
    {
        $instanceText = $Instances[0]
        for ($i = 1;$i -lt $Instances.Count;$i++)
        {
              $instanceText = $instanceText+ "," + $Instances[$i]
          }
        $publicConfig | Add-Member -MemberType NoteProperty -Name "Instances" -Value $instanceText
    }
    else  #For all instances if not specified.  hello value should be a space or *
    {
        $publicConfig | Add-Member -MemberType NoteProperty -Name "Instances" -Value " "
    }

    if ($Mode -ne $null )
    {
        $publicConfig | Add-Member -MemberType NoteProperty -Name "Mode" -Value $Mode
    }
    else
    {
        $publicConfig | Add-Member -MemberType NoteProperty -Name "Mode" -Value "Full"
    }

    #
    #we need tooget hello Sasuri from StorageAccount and containers
    #
    $context = New-AzureStorageContext -Protocol https -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

    $ContainerName = "azurelogcollectordata"
    $existingContainer = Get-AzureStorageContainer -Context $context |  Where-Object { $_.Name -like $ContainerName}
    if ($existingContainer -eq $null)
    {
        "Container ($ContainerName) doesn't exist. Creating it now.."
        New-AzureStorageContainer -Context $context -Name $ContainerName -Permission off
    }

    $ExpiryTime =  [DateTime]::Now.AddMinutes(120).ToString("o")
    $SasUri = New-AzureStorageContainerSASToken -ExpiryTime $ExpiryTime -FullUri -Name $ContainerName -Permission rwl -Context $context
    $publicConfig | Add-Member -MemberType NoteProperty -Name "SasUri" -Value $SasUri

    #
    #Add AdditionalData toocollect data from additional folders
    #
    if ($AdditionDataLocationList -ne $null )
    {
      $publicConfig | Add-Member -MemberType NoteProperty -Name "AdditionalData" -Value $AdditionDataLocationList
    }

    #
    # Convert it tooJSON format
    #
    $publicConfigJSON = $publicConfig | ConvertTo-Json
    "publicConfig is:  $publicConfigJSON"

    #we just provide a empty privateConfig object
    $privateconfig = "{
    }"

    if ($Roles -ne $null)
    {
          Set-AzureServiceExtension -Service $ServiceName -Slot $Slot -Role $Roles -ExtensionName 'AzureLogCollector' -ProviderNamespace Microsoft.WindowsAzure.Compute -PublicConfiguration $publicConfigJSON -PrivateConfiguration $privateconfig -Version 1.0 -Verbose
    }
    else
    {
          Set-AzureServiceExtension -Service $ServiceName -Slot $Slot  -ExtensionName 'AzureLogCollector' -ProviderNamespace Microsoft.WindowsAzure.Compute -PublicConfiguration $publicConfigJSON -PrivateConfiguration $privateconfig -Version 1.0 -Verbose
    }

    #
    #This is an optional step: generate a sasUri toohello container so it can be shared with other people if nened
    #
    $SasExpireTime = [DateTime]::Now.AddMinutes(120).ToString("o")
    $SasUri = New-AzureStorageContainerSASToken -ExpiryTime $ExpiryTime -FullUri -Name $ContainerName -Permission rl -Context $context
    $SasUri = $SasUri + "&restype=container&comp=list"
    Write-Output "hello container for uploaded file can be accessed using this link:`r`n$sasuri"


SetAzureVMLogCollector.ps1

    [CmdletBinding(SupportsShouldProcess = $true)]

    param (
                  [Parameter(Mandatory=$true)]
                  [string]   $ServiceName,

                  [Parameter(Mandatory=$false)]
                  [string] $VMName ,

                  [Parameter(Mandatory=$false)]
                  [string]   $Mode = 'Full',

                  [Parameter(Mandatory=$false)]
                  [string]   $StorageAccountName,

                  [Parameter(Mandatory=$false)]
                  [string]   $StorageAccountKey,

                  [Parameter(Mandatory=$false)]
                  [PSObject[]] $AdditionDataLocationList = $null
            )

    $publicConfig = New-Object PSObject
    $publicConfig | Add-Member -MemberType NoteProperty -Name "Instances" -Value "*"

    if ($Mode -ne $null )
    {
        $publicConfig | Add-Member -MemberType NoteProperty -Name "Mode" -Value $Mode
    }
    else
    {
        $publicConfig | Add-Member -MemberType NoteProperty -Name "Mode" -Value "Full"
    }

    #
    #we need tooget hello Sasuri from StorageAccount and containers
    #
    $context = New-AzureStorageContext -Protocol https -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

    $ContainerName = "azurelogcollectordata"
    $existingContainer = Get-AzureStorageContainer -Context $context |  Where-Object { $_.Name -like $ContainerName}
    if ($existingContainer -eq $null)
    {
        "Container ($ContainerName) doesn't exist. Creating it now.."
        New-AzureStorageContainer -Context $context -Name $ContainerName -Permission off
    }

    $ExpiryTime =  [DateTime]::Now.AddMinutes(90).ToString("o")
    $SasUri = New-AzureStorageContainerSASToken -ExpiryTime $ExpiryTime -FullUri -Name $ContainerName -Permission rwl -Context $context
    $publicConfig | Add-Member -MemberType NoteProperty -Name "SasUri" -Value $SasUri

    #
    #Add AdditionalData toocollect data from additional folders
    #
    if ($AdditionDataLocationList -ne $null )
    {
      $publicConfig | Add-Member -MemberType NoteProperty -Name "AdditionalData" -Value $AdditionDataLocationList
    }

    #
    # Convert it tooJSON format
    #
    $publicConfigJSON = $publicConfig | ConvertTo-Json

    Write-Output "PublicConfigurtion is: \r\n$publicConfigJSON"

    #
    #we just provide a empty privateConfig object
    #
    $privateconfig = "{
    }"

    if ($VMName -ne $null )
    {
          $VM = Get-AzureVM -ServiceName $ServiceName -Name $VMName
          $VM.VM.OSVirtualHardDisk.OS

          if ($VM.VM.OSVirtualHardDisk.OS -like '*Windows*')
          {
                Set-AzureVMExtension -VM $VM -ExtensionName "AzureLogCollector" -Publisher Microsoft.WindowsAzure.Compute -PublicConfiguration $publicConfigJSON -PrivateConfiguration $privateconfig -Version 1.* | Update-AzureVM -Verbose

                #
                #We will check hello VM status toofind if operation by extension has been completed or not. hello completion of hello operation,either succeed or fail, can be indicated by
                #hello presence of SubstatusList field.
                #
                $Completed = $false
                while ($Completed -ne $true)
                {
                        $VM = Get-AzureVM -ServiceName $ServiceName -Name $VMName
                        $status = $VM.ResourceExtensionStatusList | Where-Object {$_.HandlerName -eq "Microsoft.WindowsAzure.Compute.AzureLogCollector"}

                        if ( ($status.Code -ne 0) -and ($status.Status -like '*error*'))
                        {
                            Write-Output "Error status is returned: $($Status.ExtensionSettingStatus.FormattedMessage.Message)."
                              $Completed = $true
                        }
                        elseif (($status.ExtensionSettingStatus.SubstatusList -eq $null -or $status.ExtensionSettingStatus.SubstatusList.Count -lt 1))
                        {
                              $Completed = $false
                              Write-Output "Waiting for operation toocomplete..."
                        }
                        else
                        {
                              $Completed = $true
                              Write-Output "Operation completed."

                        $UploadedFileUri = $Status.ExtensionSettingStatus.SubStatusList[0].FormattedMessage.Message
                              $blob = New-Object Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob($UploadedFileUri)

                      #
                            # This is an optional step:  For easier access toohello file, we can generate a read-only SasUri directly toohello file
                              #
                              $ExpiryTimeRead =  [DateTime]::Now.AddMinutes(120).ToString("o")
                              $ReadSasUri = New-AzureStorageBlobSASToken -ExpiryTime $ExpiryTimeRead  -FullUri  -Blob  $blob.name -Container $blob.Container.Name -Permission r -Context $context

                            Write-Output "hello uploaded file can be accessed using this link: $ReadSasUri"

                              #
                              #This is an optional step:  Remove hello extension after we are done
                              #
                              Get-AzureVM -ServiceName $ServiceName -Name $VMName | Set-AzureVMExtension -Publisher Microsoft.WindowsAzure.Compute -ExtensionName "AzureLogCollector" -Version 1.* -Uninstall | Update-AzureVM -Verbose

                        }
                        Start-Sleep -s 5
                }
          }
          else
          {
              Write-Output "VM OS Type is not Windows, hello extension cannot be enabled"
          }

    }
    else
    {
      Write-Output "VM name is not specified, hello extension cannot be enabled"
    }

## <a name="next-steps"></a>Další kroky
Nyní můžete prozkoumat nebo kopírování protokolů z jednoho umístění velmi jednoduché.

