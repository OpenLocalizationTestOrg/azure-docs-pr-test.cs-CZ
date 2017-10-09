---
title: "aaaDesired šablony Resource Manageru konfigurace stavu | Microsoft Docs"
description: "Definice šablony Resource Manageru pro konfigurace požadovaného stavu v Azure s příklady a řešení potíží"
services: virtual-machines-windows
documentationcenter: 
author: zjalexander
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
keywords: 
ms.assetid: b5402e5a-1768-4075-8c19-b7f7402687af
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: na
ms.date: 09/15/2016
ms.author: zachal
ms.openlocfilehash: fc47ac149b1179d9305797eaa8ed8a83c0958c37
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="windows-vmss-and-desired-state-configuration-with-azure-resource-manager-templates"></a><span data-ttu-id="84514-103">Windows VMSS a konfigurace požadovaného stavu pomocí šablon Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="84514-103">Windows VMSS and Desired State Configuration with Azure Resource Manager templates</span></span>
<span data-ttu-id="84514-104">Tento článek popisuje hello šablony Resource Manageru pro hello [obslužná rutina rozšíření konfigurace požadovaného stavu](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="84514-104">This article describes hello Resource Manager template for hello [Desired State Configuration extension handler](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> 

## <a name="template-example-for-a-windows-vm"></a><span data-ttu-id="84514-105">Příklad šablony pro virtuální počítač s Windows</span><span class="sxs-lookup"><span data-stu-id="84514-105">Template example for a Windows VM</span></span>
<span data-ttu-id="84514-106">Hello následující fragment kódu přejde do hello oddíl hello šablony.</span><span class="sxs-lookup"><span data-stu-id="84514-106">hello following snippet goes into hello Resource section of hello template.</span></span>

```json
            "name": "Microsoft.Powershell.DSC",
            "type": "extensions",
             "location": "[resourceGroup().location]",
             "apiVersion": "2015-06-15",
             "dependsOn": [
                  "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
              ],
              "properties": {
                  "publisher": "Microsoft.Powershell",
                  "type": "DSC",
                  "typeHandlerVersion": "2.20",
                  "autoUpgradeMinorVersion": true,
                  "forceUpdateTag": "[parameters('dscExtensionUpdateTagVersion')]",
                  "settings": {
                      "configuration": {
                          "url": "[concat(parameters('_artifactsLocation'), '/', variables('dscExtensionArchiveFolder'), '/', variables('dscExtensionArchiveFileName'))]",
                          "script": "dscExtension.ps1",
                          "function": "Main"
                      },
                      "configurationArguments": {
                          "nodeName": "[variables('vmName')]"
                      }
                  },
                  "protectedSettings": {
                      "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                  }
              }

```

## <a name="template-example-for-windows-vmss"></a><span data-ttu-id="84514-107">Příklad šablony Windows VMSS</span><span class="sxs-lookup"><span data-stu-id="84514-107">Template example for Windows VMSS</span></span>
<span data-ttu-id="84514-108">Oddíl "vlastnosti" hello "VirtualMachineProfile", "extensionProfile" atribut má VMSS uzel.</span><span class="sxs-lookup"><span data-stu-id="84514-108">A VMSS node has a "properties" section with hello "VirtualMachineProfile", "extensionProfile" attribute.</span></span> <span data-ttu-id="84514-109">V části "rozšíření" se přidá DSC.</span><span class="sxs-lookup"><span data-stu-id="84514-109">DSC is added under "extensions".</span></span> 

```json
"extensionProfile": {
            "extensions": [
                {
                    "name": "Microsoft.Powershell.DSC",
                    "properties": {
                        "publisher": "Microsoft.Powershell",
                        "type": "DSC",
                        "typeHandlerVersion": "2.20",
                        "autoUpgradeMinorVersion": true,
                        "forceUpdateTag": "[parameters('DscExtensionUpdateTagVersion')]",
                        "settings": {
                            "configuration": {
                                "url": "[concat(parameters('_artifactsLocation'), '/', variables('DscExtensionArchiveFolder'), '/', variables('DscExtensionArchiveFileName'))]",
                                "script": "DscExtension.ps1",
                                "function": "Main"
                            },
                            "configurationArguments": {
                                "nodeName": "localhost"
                            }
                        },
                        "protectedSettings": {
                            "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                        }
                    }
                }
            ]
        }
```

## <a name="detailed-settings-information"></a><span data-ttu-id="84514-110">Informace o podrobné nastavení</span><span class="sxs-lookup"><span data-stu-id="84514-110">Detailed Settings Information</span></span>
<span data-ttu-id="84514-111">Hello následující schéma je pro část nastavení hello hello rozšíření Azure DSC v šablonu Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="84514-111">hello following schema is for hello settings portion of hello Azure DSC extension in an Azure Resource Manager template.</span></span>

```json

"settings": {
  "wmfVersion": "latest",
  "configuration": {
    "url": "http://validURLToConfigLocation",
    "script": "ConfigurationScript.ps1",
    "function": "ConfigurationFunction"
  },
  "configurationArguments": {
    "argument1": "Value1",
    "argument2": "Value2"
  },
  "configurationData": {
    "url": "https://foo.psd1"
  },
  "privacy": {
    "dataCollection": "enable"
  },
  "advancedOptions": {
    "downloadMappings": {
      "customWmfLocation": "http://myWMFlocation"
    }
  } 
},
"protectedSettings": {
  "configurationArguments": {
    "parameterOfTypePSCredential1": {
      "userName": "UsernameValue1",
      "password": "PasswordValue1"
    },
    "parameterOfTypePSCredential2": {
      "userName": "UsernameValue2",
      "password": "PasswordValue2"
    }
  },
  "configurationUrlSasToken": "?g!bber1sht0k3n",
  "configurationDataUrlSasToken": "?dataAcC355T0k3N"
}

```

## <a name="details"></a><span data-ttu-id="84514-112">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="84514-112">Details</span></span>
| <span data-ttu-id="84514-113">Název vlastnosti</span><span class="sxs-lookup"><span data-stu-id="84514-113">Property Name</span></span> | <span data-ttu-id="84514-114">Typ</span><span class="sxs-lookup"><span data-stu-id="84514-114">Type</span></span> | <span data-ttu-id="84514-115">Popis</span><span class="sxs-lookup"><span data-stu-id="84514-115">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="84514-116">settings.wmfVersion</span><span class="sxs-lookup"><span data-stu-id="84514-116">settings.wmfVersion</span></span> |<span data-ttu-id="84514-117">Řetězec</span><span class="sxs-lookup"><span data-stu-id="84514-117">string</span></span> |<span data-ttu-id="84514-118">Určuje verzi hello hello Windows Management Framework, který by měly být nainstalovány na vašem virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="84514-118">Specifies hello version of hello Windows Management Framework that should be installed on your VM.</span></span> <span data-ttu-id="84514-119">Nastavení této vlastnosti too'latest se nainstaluje hello nejaktuálnější verzi WMF.</span><span class="sxs-lookup"><span data-stu-id="84514-119">Setting this property too'latest' installs hello most updated version of WMF.</span></span> <span data-ttu-id="84514-120">Hello pouze aktuální možné hodnoty pro tuto vlastnost jsou **'4.0', '5.0', ' 5.0PP' a 'nejnovější'**.</span><span class="sxs-lookup"><span data-stu-id="84514-120">hello only current possible values for this property are **'4.0', '5.0', '5.0PP', and 'latest'**.</span></span> <span data-ttu-id="84514-121">Tyto hodnoty jsou tooupdates subjektu.</span><span class="sxs-lookup"><span data-stu-id="84514-121">These possible values are subject tooupdates.</span></span> <span data-ttu-id="84514-122">Hello výchozí hodnota je 'nejnovější'.</span><span class="sxs-lookup"><span data-stu-id="84514-122">hello default value is 'latest'.</span></span> |
| <span data-ttu-id="84514-123">Settings.Configuration.URL</span><span class="sxs-lookup"><span data-stu-id="84514-123">settings.configuration.url</span></span> |<span data-ttu-id="84514-124">Řetězec</span><span class="sxs-lookup"><span data-stu-id="84514-124">string</span></span> |<span data-ttu-id="84514-125">Určuje umístění hello adresu URL, ze které toodownload souboru zip konfigurace DSC.</span><span class="sxs-lookup"><span data-stu-id="84514-125">Specifies hello URL location from which toodownload your DSC configuration zip file.</span></span> <span data-ttu-id="84514-126">Pokud zadaná adresa URL hello vyžaduje SAS token pro přístup, je třeba tooset hello protectedSettings.configurationUrlSasToken toohello hodnota vlastnosti tokenu SAS.</span><span class="sxs-lookup"><span data-stu-id="84514-126">If hello URL provided requires a SAS token for access, you need tooset hello protectedSettings.configurationUrlSasToken property toohello value of your SAS token.</span></span> <span data-ttu-id="84514-127">Tato vlastnost je vyžadována, pokud jsou definovány settings.configuration.script nebo settings.configuration.function.</span><span class="sxs-lookup"><span data-stu-id="84514-127">This property is required if settings.configuration.script and/or settings.configuration.function are defined.</span></span> |
| <span data-ttu-id="84514-128">Settings.Configuration.Script</span><span class="sxs-lookup"><span data-stu-id="84514-128">settings.configuration.script</span></span> |<span data-ttu-id="84514-129">Řetězec</span><span class="sxs-lookup"><span data-stu-id="84514-129">string</span></span> |<span data-ttu-id="84514-130">Určuje název souboru hello hello skriptu, který obsahuje hello definici konfigurace DSC.</span><span class="sxs-lookup"><span data-stu-id="84514-130">Specifies hello file name of hello script that contains hello definition of your DSC configuration.</span></span> <span data-ttu-id="84514-131">Tento skript musí být v kořenové složce hello hello zip soubor stažený z adresy URL hello určený vlastností configuration.url hello.</span><span class="sxs-lookup"><span data-stu-id="84514-131">This script must be in hello root folder of hello zip file downloaded from hello URL specified by hello configuration.url property.</span></span> <span data-ttu-id="84514-132">Tato vlastnost je vyžadována, pokud jsou definovány settings.configuration.url nebo settings.configuration.script.</span><span class="sxs-lookup"><span data-stu-id="84514-132">This property is required if settings.configuration.url and/or settings.configuration.script are defined.</span></span> |
| <span data-ttu-id="84514-133">Settings.Configuration.Function</span><span class="sxs-lookup"><span data-stu-id="84514-133">settings.configuration.function</span></span> |<span data-ttu-id="84514-134">Řetězec</span><span class="sxs-lookup"><span data-stu-id="84514-134">string</span></span> |<span data-ttu-id="84514-135">Určuje název hello konfigurace DSC.</span><span class="sxs-lookup"><span data-stu-id="84514-135">Specifies hello name of your DSC configuration.</span></span> <span data-ttu-id="84514-136">Hello konfigurace s názvem musí být součástí skriptu hello definované configuration.script.</span><span class="sxs-lookup"><span data-stu-id="84514-136">hello configuration named must be contained in hello script defined by configuration.script.</span></span> <span data-ttu-id="84514-137">Tato vlastnost je vyžadována, pokud jsou definovány settings.configuration.url nebo settings.configuration.function.</span><span class="sxs-lookup"><span data-stu-id="84514-137">This property is required if settings.configuration.url and/or settings.configuration.function are defined.</span></span> |
| <span data-ttu-id="84514-138">settings.configurationArguments</span><span class="sxs-lookup"><span data-stu-id="84514-138">settings.configurationArguments</span></span> |<span data-ttu-id="84514-139">Kolekce</span><span class="sxs-lookup"><span data-stu-id="84514-139">Collection</span></span> |<span data-ttu-id="84514-140">Definuje žádné parametry, které byste chtěli konfigurace DSC tooyour toopass.</span><span class="sxs-lookup"><span data-stu-id="84514-140">Defines any parameters you would like toopass tooyour DSC configuration.</span></span> <span data-ttu-id="84514-141">Tato vlastnost není zašifrován.</span><span class="sxs-lookup"><span data-stu-id="84514-141">This property is not encrypted.</span></span> |
| <span data-ttu-id="84514-142">settings.configurationData.url</span><span class="sxs-lookup"><span data-stu-id="84514-142">settings.configurationData.url</span></span> |<span data-ttu-id="84514-143">Řetězec</span><span class="sxs-lookup"><span data-stu-id="84514-143">string</span></span> |<span data-ttu-id="84514-144">Určuje adresu URL hello z které toodownload svá konfigurační data (.psd1) souboru toouse jako vstup pro konfiguraci DSC.</span><span class="sxs-lookup"><span data-stu-id="84514-144">Specifies hello URL from which toodownload your configuration data (.psd1) file toouse as input for your DSC configuration.</span></span> <span data-ttu-id="84514-145">Pokud zadaná adresa URL hello vyžaduje SAS token pro přístup, je třeba tooset hello protectedSettings.configurationDataUrlSasToken toohello hodnota vlastnosti tokenu SAS.</span><span class="sxs-lookup"><span data-stu-id="84514-145">If hello URL provided requires a SAS token for access, you need tooset hello protectedSettings.configurationDataUrlSasToken property toohello value of your SAS token.</span></span> |
| <span data-ttu-id="84514-146">settings.privacy.dataEnabled</span><span class="sxs-lookup"><span data-stu-id="84514-146">settings.privacy.dataEnabled</span></span> |<span data-ttu-id="84514-147">Řetězec</span><span class="sxs-lookup"><span data-stu-id="84514-147">string</span></span> |<span data-ttu-id="84514-148">Povolí nebo zakáže telemetrie kolekce.</span><span class="sxs-lookup"><span data-stu-id="84514-148">Enables or disables telemetry collection.</span></span> <span data-ttu-id="84514-149">Hello pouze možné hodnoty pro tuto vlastnost jsou **povolit, "Zakázat", ", nebo $null**.</span><span class="sxs-lookup"><span data-stu-id="84514-149">hello only possible values for this property are **'Enable', 'Disable', '', or $null**.</span></span> <span data-ttu-id="84514-150">Umožňuje telemetrie, a tato vlastnost prázdná nebo mít hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="84514-150">Leaving this property blank or null enables telemetry.</span></span> <span data-ttu-id="84514-151">Hello výchozí hodnota je ".</span><span class="sxs-lookup"><span data-stu-id="84514-151">hello default value is ''.</span></span> [<span data-ttu-id="84514-152">Další informace</span><span class="sxs-lookup"><span data-stu-id="84514-152">More Information</span></span>](https://blogs.msdn.microsoft.com/powershell/2016/02/02/azure-dsc-extension-data-collection-2/) |
| <span data-ttu-id="84514-153">settings.advancedOptions.downloadMappings</span><span class="sxs-lookup"><span data-stu-id="84514-153">settings.advancedOptions.downloadMappings</span></span> |<span data-ttu-id="84514-154">Kolekce</span><span class="sxs-lookup"><span data-stu-id="84514-154">Collection</span></span> |<span data-ttu-id="84514-155">Určuje alternativní umístění, ze které toodownload hello WMF.</span><span class="sxs-lookup"><span data-stu-id="84514-155">Defines alternate locations from which toodownload hello WMF.</span></span> [<span data-ttu-id="84514-156">Další informace</span><span class="sxs-lookup"><span data-stu-id="84514-156">More Information</span></span>](http://blogs.msdn.com/b/powershell/archive/2015/10/21/azure-dsc-extension-2-2-amp-how-to-map-downloads-of-the-extension-dependencies-to-your-own-location.aspx) |
| <span data-ttu-id="84514-157">protectedSettings.configurationArguments</span><span class="sxs-lookup"><span data-stu-id="84514-157">protectedSettings.configurationArguments</span></span> |<span data-ttu-id="84514-158">Kolekce</span><span class="sxs-lookup"><span data-stu-id="84514-158">Collection</span></span> |<span data-ttu-id="84514-159">Definuje žádné parametry, které byste chtěli konfigurace DSC tooyour toopass.</span><span class="sxs-lookup"><span data-stu-id="84514-159">Defines any parameters you would like toopass tooyour DSC configuration.</span></span> <span data-ttu-id="84514-160">Tato vlastnost je zašifrován.</span><span class="sxs-lookup"><span data-stu-id="84514-160">This property is encrypted.</span></span> |
| <span data-ttu-id="84514-161">protectedSettings.configurationUrlSasToken</span><span class="sxs-lookup"><span data-stu-id="84514-161">protectedSettings.configurationUrlSasToken</span></span> |<span data-ttu-id="84514-162">Řetězec</span><span class="sxs-lookup"><span data-stu-id="84514-162">string</span></span> |<span data-ttu-id="84514-163">Určuje hello SAS token tooaccess hello URL definované configuration.url.</span><span class="sxs-lookup"><span data-stu-id="84514-163">Specifies hello SAS token tooaccess hello URL defined by configuration.url.</span></span> <span data-ttu-id="84514-164">Tato vlastnost je zašifrován.</span><span class="sxs-lookup"><span data-stu-id="84514-164">This property is encrypted.</span></span> |
| <span data-ttu-id="84514-165">protectedSettings.configurationDataUrlSasToken</span><span class="sxs-lookup"><span data-stu-id="84514-165">protectedSettings.configurationDataUrlSasToken</span></span> |<span data-ttu-id="84514-166">Řetězec</span><span class="sxs-lookup"><span data-stu-id="84514-166">string</span></span> |<span data-ttu-id="84514-167">Určuje hello SAS token tooaccess hello URL definované configurationData.url.</span><span class="sxs-lookup"><span data-stu-id="84514-167">Specifies hello SAS token tooaccess hello URL defined by configurationData.url.</span></span> <span data-ttu-id="84514-168">Tato vlastnost je zašifrován.</span><span class="sxs-lookup"><span data-stu-id="84514-168">This property is encrypted.</span></span> |

## <a name="settings-vs-protectedsettings"></a><span data-ttu-id="84514-169">Nastavení vs. ProtectedSettings</span><span class="sxs-lookup"><span data-stu-id="84514-169">Settings vs. ProtectedSettings</span></span>
<span data-ttu-id="84514-170">Všechna nastavení se ukládají do textového souboru nastavení na hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="84514-170">All settings are saved in a settings text file on hello VM.</span></span>
<span data-ttu-id="84514-171">Vlastnosti v části "nastavení" jsou veřejné vlastnosti, protože nejsou šifrovány hello nastavení textového souboru.</span><span class="sxs-lookup"><span data-stu-id="84514-171">Properties under 'settings' are public properties because they are not encrypted in hello settings text file.</span></span>
<span data-ttu-id="84514-172">Vlastnosti v části 'protectedSettings' jsou šifrované pomocí certifikátu a nejsou zobrazeny ve formátu prostého textu v tomto souboru na hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="84514-172">Properties under 'protectedSettings' are encrypted with a certificate and are not shown in plain text in this file on hello VM.</span></span>

<span data-ttu-id="84514-173">Pokud konfigurace hello potřebuje přihlašovací údaje, můžou být součástí protectedSettings:</span><span class="sxs-lookup"><span data-stu-id="84514-173">If hello configuration needs credentials, they can be included in protectedSettings:</span></span>

```json
"protectedSettings": {
    "configurationArguments": {
        "parameterOfTypePSCredential1": {
               "userName": "UsernameValue1",
               "password": "PasswordValue1"
        }
    }
}
```

## <a name="example"></a><span data-ttu-id="84514-174">Příklad</span><span class="sxs-lookup"><span data-stu-id="84514-174">Example</span></span>
<span data-ttu-id="84514-175">Hello následující příklad je odvozena z hello oddílu hello "Začínáme" [stránku přehled obslužná rutina rozšíření DSC](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="84514-175">hello following example derives from hello "Getting Started" section of hello [DSC Extension Handler Overview page](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
<span data-ttu-id="84514-176">Tento příklad používá šablony Resource Manager místo rutiny toodeploy hello rozšíření.</span><span class="sxs-lookup"><span data-stu-id="84514-176">This example uses Resource Manager templates instead of cmdlets toodeploy hello extension.</span></span> <span data-ttu-id="84514-177">Uložte konfiguraci "IisInstall.ps1" hello, jeho umístění. ZIP – soubory a nahrávání hello soubor v přístupné adresy URL.</span><span class="sxs-lookup"><span data-stu-id="84514-177">Save hello "IisInstall.ps1" configuration, place it in a .ZIP file, and upload hello file in an accessible URL.</span></span> <span data-ttu-id="84514-178">Tento příklad používá úložiště objektů blob v Azure, ale je možné toodownload. Soubory ZIP libovolný odkudkoli.</span><span class="sxs-lookup"><span data-stu-id="84514-178">This example uses Azure blob storage, but it is possible toodownload .ZIP files from any arbitrary location.</span></span>

<span data-ttu-id="84514-179">V šabloně Azure Resource Manager hello hello následující kód dá pokyn hello virtuálních počítačů toodownload hello správný soubor a spusťte hello odpovídající funkce prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="84514-179">In hello Azure Resource Manager template, hello following code instructs hello VM toodownload hello correct file and run hello appropriate PowerShell function:</span></span>

```json
"settings": {
    "configuration": {
        "url": "https://demo.blob.core.windows.net/",
        "script": "IisInstall.ps1",
        "function": "IISInstall"
    }
    } 
},
"protectedSettings": {
    "configurationUrlSasToken": "odLPL/U1p9lvcnp..."
}
```

## <a name="updating-from-hello-previous-format"></a><span data-ttu-id="84514-180">Aktualizace z hello předchozí formátu</span><span class="sxs-lookup"><span data-stu-id="84514-180">Updating from hello Previous Format</span></span>
<span data-ttu-id="84514-181">Všechna nastavení v hello předchozí formátu (obsahující hello veřejné vlastnosti ModulesUrl ConfigurationFunction, SasToken nebo vlastnosti) automaticky přizpůsobit toohello současném formátu a spusťte stejně jako před.</span><span class="sxs-lookup"><span data-stu-id="84514-181">Any settings in hello previous format (containing hello public properties ModulesUrl, ConfigurationFunction, SasToken, or Properties) automatically adapt toohello current format and run just as they did before.</span></span>

<span data-ttu-id="84514-182">Následující schéma Hello je co hello předchozí nastavení schématu hledá jako:</span><span class="sxs-lookup"><span data-stu-id="84514-182">hello following schema is what hello previous settings schema looked like:</span></span>

```json
"settings": {
    "WMFVersion": "latest",
    "ModulesUrl": "https://UrlToZipContainingConfigurationScript.ps1.zip",
    "SasToken": "SAS Token if ModulesUrl points tooprivate Azure Blob Storage",
    "ConfigurationFunction": "ConfigurationScript.ps1\\ConfigurationFunction",
    "Properties":  {
        "ParameterToConfigurationFunction1":  "Value1",
        "ParameterToConfigurationFunction2":  "Value2",
        "ParameterOfTypePSCredential1": {
            "UserName": "UsernameValue1",
            "Password": "PrivateSettingsRef:Key1" 
        },
        "ParameterOfTypePSCredential2": {
            "UserName": "UsernameValue2",
            "Password": "PrivateSettingsRef:Key2"
        }
    }
},
"protectedSettings": { 
    "Items": {
        "Key1": "PasswordValue1",
        "Key2": "PasswordValue2"
    },
    "DataBlobUri": "https://UrlToConfigurationDataWithOptionalSasToken.psd1"
}
```

<span data-ttu-id="84514-183">Zde je, jak předchozí formátu hello přizpůsobuje toohello aktuální formát:</span><span class="sxs-lookup"><span data-stu-id="84514-183">Here's how hello previous format adapts toohello current format:</span></span>

| <span data-ttu-id="84514-184">Název vlastnosti</span><span class="sxs-lookup"><span data-stu-id="84514-184">Property Name</span></span> | <span data-ttu-id="84514-185">Předchozí ekvivalent schématu</span><span class="sxs-lookup"><span data-stu-id="84514-185">Previous Schema Equivalent</span></span> |
| --- | --- |
| <span data-ttu-id="84514-186">settings.wmfVersion</span><span class="sxs-lookup"><span data-stu-id="84514-186">settings.wmfVersion</span></span> |<span data-ttu-id="84514-187">nastavení. WMFVersion</span><span class="sxs-lookup"><span data-stu-id="84514-187">settings.WMFVersion</span></span> |
| <span data-ttu-id="84514-188">Settings.Configuration.URL</span><span class="sxs-lookup"><span data-stu-id="84514-188">settings.configuration.url</span></span> |<span data-ttu-id="84514-189">nastavení. ModulesUrl</span><span class="sxs-lookup"><span data-stu-id="84514-189">settings.ModulesUrl</span></span> |
| <span data-ttu-id="84514-190">Settings.Configuration.Script</span><span class="sxs-lookup"><span data-stu-id="84514-190">settings.configuration.script</span></span> |<span data-ttu-id="84514-191">První část nastavení. ConfigurationFunction (před '\\\\.)</span><span class="sxs-lookup"><span data-stu-id="84514-191">First part of settings.ConfigurationFunction (before '\\\\')</span></span> |
| <span data-ttu-id="84514-192">Settings.Configuration.Function</span><span class="sxs-lookup"><span data-stu-id="84514-192">settings.configuration.function</span></span> |<span data-ttu-id="84514-193">Druhá část nastavení. ConfigurationFunction (po '\\\\.)</span><span class="sxs-lookup"><span data-stu-id="84514-193">Second part of settings.ConfigurationFunction (after '\\\\')</span></span> |
| <span data-ttu-id="84514-194">settings.configurationArguments</span><span class="sxs-lookup"><span data-stu-id="84514-194">settings.configurationArguments</span></span> |<span data-ttu-id="84514-195">nastavení. Vlastnosti</span><span class="sxs-lookup"><span data-stu-id="84514-195">settings.Properties</span></span> |
| <span data-ttu-id="84514-196">settings.configurationData.url</span><span class="sxs-lookup"><span data-stu-id="84514-196">settings.configurationData.url</span></span> |<span data-ttu-id="84514-197">protectedSettings.DataBlobUri (bez tokenu SAS)</span><span class="sxs-lookup"><span data-stu-id="84514-197">protectedSettings.DataBlobUri (without SAS token)</span></span> |
| <span data-ttu-id="84514-198">settings.privacy.dataEnabled</span><span class="sxs-lookup"><span data-stu-id="84514-198">settings.privacy.dataEnabled</span></span> |<span data-ttu-id="84514-199">nastavení. Privacy.DataEnabled</span><span class="sxs-lookup"><span data-stu-id="84514-199">settings.Privacy.DataEnabled</span></span> |
| <span data-ttu-id="84514-200">settings.advancedOptions.downloadMappings</span><span class="sxs-lookup"><span data-stu-id="84514-200">settings.advancedOptions.downloadMappings</span></span> |<span data-ttu-id="84514-201">nastavení. AdvancedOptions.DownloadMappings</span><span class="sxs-lookup"><span data-stu-id="84514-201">settings.AdvancedOptions.DownloadMappings</span></span> |
| <span data-ttu-id="84514-202">protectedSettings.configurationArguments</span><span class="sxs-lookup"><span data-stu-id="84514-202">protectedSettings.configurationArguments</span></span> |<span data-ttu-id="84514-203">protectedSettings.Properties</span><span class="sxs-lookup"><span data-stu-id="84514-203">protectedSettings.Properties</span></span> |
| <span data-ttu-id="84514-204">protectedSettings.configurationUrlSasToken</span><span class="sxs-lookup"><span data-stu-id="84514-204">protectedSettings.configurationUrlSasToken</span></span> |<span data-ttu-id="84514-205">nastavení. SasToken</span><span class="sxs-lookup"><span data-stu-id="84514-205">settings.SasToken</span></span> |
| <span data-ttu-id="84514-206">protectedSettings.configurationDataUrlSasToken</span><span class="sxs-lookup"><span data-stu-id="84514-206">protectedSettings.configurationDataUrlSasToken</span></span> |<span data-ttu-id="84514-207">Token SAS z protectedSettings.DataBlobUri</span><span class="sxs-lookup"><span data-stu-id="84514-207">SAS token from protectedSettings.DataBlobUri</span></span> |

## <a name="troubleshooting---error-code-1100"></a><span data-ttu-id="84514-208">Řešení potíží – kód chyby 1100</span><span class="sxs-lookup"><span data-stu-id="84514-208">Troubleshooting - Error Code 1100</span></span>
<span data-ttu-id="84514-209">Kód chyby 1100 označuje, že došlo k potížím s hello rozšíření toohello DSC vstup uživatele.</span><span class="sxs-lookup"><span data-stu-id="84514-209">Error Code 1100 indicates that there is a problem with hello user input toohello DSC extension.</span></span>
<span data-ttu-id="84514-210">Tyto chyby textu Hello je proměnná a může se změnit.</span><span class="sxs-lookup"><span data-stu-id="84514-210">hello text of these errors is variable and may change.</span></span>
<span data-ttu-id="84514-211">Zde jsou některé hello chyb, které může spustit do a jak můžete opravit je.</span><span class="sxs-lookup"><span data-stu-id="84514-211">Here are some of hello errors you may run into and how you can fix them.</span></span>

### <a name="invalid-values"></a><span data-ttu-id="84514-212">Neplatné hodnoty</span><span class="sxs-lookup"><span data-stu-id="84514-212">Invalid Values</span></span>
<span data-ttu-id="84514-213">"Je Privacy.dataCollection: {0}.</span><span class="sxs-lookup"><span data-stu-id="84514-213">"Privacy.dataCollection is '{0}'.</span></span> <span data-ttu-id="84514-214">Hello pouze možné hodnoty jsou ","Zapnout"a"Zakázat"" "je WmfVersion: {0}.</span><span class="sxs-lookup"><span data-stu-id="84514-214">hello only possible values are '', 'Enable', and 'Disable'" "WmfVersion is '{0}'.</span></span> <span data-ttu-id="84514-215">Pouze možné hodnoty jsou...</span><span class="sxs-lookup"><span data-stu-id="84514-215">Only possible values are …</span></span> <span data-ttu-id="84514-216">a 'nejnovější' "</span><span class="sxs-lookup"><span data-stu-id="84514-216">and 'latest'"</span></span>

<span data-ttu-id="84514-217">Problém: Zadaná hodnota není povolena.</span><span class="sxs-lookup"><span data-stu-id="84514-217">Problem: A provided value is not allowed.</span></span>

<span data-ttu-id="84514-218">Řešení: Změňte hello neplatná hodnota tooa platnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="84514-218">Solution: Change hello invalid value tooa valid value.</span></span> <span data-ttu-id="84514-219">Najdete hello tabulce v části Podrobnosti o hello.</span><span class="sxs-lookup"><span data-stu-id="84514-219">See hello table in hello Details section.</span></span>

### <a name="invalid-url"></a><span data-ttu-id="84514-220">Neplatná adresa URL</span><span class="sxs-lookup"><span data-stu-id="84514-220">Invalid URL</span></span>
<span data-ttu-id="84514-221">"Je ConfigurationData.url: {0}.</span><span class="sxs-lookup"><span data-stu-id="84514-221">"ConfigurationData.url is '{0}'.</span></span> <span data-ttu-id="84514-222">Toto není platná adresa URL.""je DataBlobUri: {0}.</span><span class="sxs-lookup"><span data-stu-id="84514-222">This is not a valid URL" "DataBlobUri is '{0}'.</span></span> <span data-ttu-id="84514-223">Toto není platná adresa URL.""je Configuration.url: {0}.</span><span class="sxs-lookup"><span data-stu-id="84514-223">This is not a valid URL" "Configuration.url is '{0}'.</span></span> <span data-ttu-id="84514-224">Toto není platná adresa URL."</span><span class="sxs-lookup"><span data-stu-id="84514-224">This is not a valid URL"</span></span>

<span data-ttu-id="84514-225">Problém: A, pokud že adresa URL není platná.</span><span class="sxs-lookup"><span data-stu-id="84514-225">Problem: A provided URL is not valid.</span></span>

<span data-ttu-id="84514-226">Řešení: Zkontrolujte všechny zadané URL.</span><span class="sxs-lookup"><span data-stu-id="84514-226">Solution: Check all your provided URLs.</span></span> <span data-ttu-id="84514-227">Ujistěte se, že všechny adresy URL přeložit umístění toovalid, kterým rozšíření hello přístup na vzdáleném počítači hello.</span><span class="sxs-lookup"><span data-stu-id="84514-227">Make sure all URLs resolve toovalid locations that hello extension can access on hello remote machine.</span></span>

### <a name="invalid-configurationargument-type"></a><span data-ttu-id="84514-228">Neplatný typ ConfigurationArgument</span><span class="sxs-lookup"><span data-stu-id="84514-228">Invalid ConfigurationArgument Type</span></span>
<span data-ttu-id="84514-229">"Neplatný configurationArguments typu {0}"</span><span class="sxs-lookup"><span data-stu-id="84514-229">"Invalid configurationArguments type {0}"</span></span>

<span data-ttu-id="84514-230">Problém: hello ConfigurationArguments vlastnost nelze vyřešit tooa zatřiďovací tabulky objektu.</span><span class="sxs-lookup"><span data-stu-id="84514-230">Problem: hello ConfigurationArguments property cannot resolve tooa Hashtable object.</span></span> 

<span data-ttu-id="84514-231">Řešení: Zkontrolujte vaši ConfigurationArguments vlastnost zatřiďovací tabulku.</span><span class="sxs-lookup"><span data-stu-id="84514-231">Solution: Make your ConfigurationArguments property a Hashtable.</span></span> <span data-ttu-id="84514-232">Použijte formát hello uvedené v předcházející příklad hello.</span><span class="sxs-lookup"><span data-stu-id="84514-232">Follow hello format provided in hello preceeding example.</span></span> <span data-ttu-id="84514-233">Sledujte uvozovky, čárky a složené závorky.</span><span class="sxs-lookup"><span data-stu-id="84514-233">Watch out for quotes, commas, and braces.</span></span>

### <a name="duplicate-configurationarguments"></a><span data-ttu-id="84514-234">Duplicitní ConfigurationArguments</span><span class="sxs-lookup"><span data-stu-id="84514-234">Duplicate ConfigurationArguments</span></span>
<span data-ttu-id="84514-235">"Nalezena ve veřejných i chráněný configurationArguments duplicitní argumenty {0}."</span><span class="sxs-lookup"><span data-stu-id="84514-235">"Found duplicate arguments '{0}' in both public and protected configurationArguments"</span></span>

<span data-ttu-id="84514-236">Problém: hello ConfigurationArguments v nastavení veřejných a hello ConfigurationArguments v chráněných nastavení obsahovat vlastnosti s hello stejný název.</span><span class="sxs-lookup"><span data-stu-id="84514-236">Problem: hello ConfigurationArguments in public settings and hello ConfigurationArguments in protected settings contain properties with hello same name.</span></span>

<span data-ttu-id="84514-237">Řešení: Odeberte jedno z hello duplicitní vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="84514-237">Solution: Remove one of hello duplicate properties.</span></span>

### <a name="missing-properties"></a><span data-ttu-id="84514-238">Chybí vlastnosti</span><span class="sxs-lookup"><span data-stu-id="84514-238">Missing Properties</span></span>
<span data-ttu-id="84514-239">"Configuration.function vyžaduje, aby byl zadán configuration.url nebo configuration.module"</span><span class="sxs-lookup"><span data-stu-id="84514-239">"Configuration.function requires that configuration.url or configuration.module is specified"</span></span>

<span data-ttu-id="84514-240">"Configuration.url vyžaduje, aby byl že tento configuration.script je zadán"</span><span class="sxs-lookup"><span data-stu-id="84514-240">"Configuration.url requires that configuration.script is specified"</span></span>

<span data-ttu-id="84514-241">"Configuration.script vyžaduje, aby byl že tento configuration.url je zadán"</span><span class="sxs-lookup"><span data-stu-id="84514-241">"Configuration.script requires that configuration.url is specified"</span></span>

<span data-ttu-id="84514-242">"Configuration.url vyžaduje, aby byl že tento configuration.function je zadán"</span><span class="sxs-lookup"><span data-stu-id="84514-242">"Configuration.url requires that configuration.function is specified"</span></span>

<span data-ttu-id="84514-243">"ConfigurationUrlSasToken vyžaduje, aby byl že tento configuration.url je zadán"</span><span class="sxs-lookup"><span data-stu-id="84514-243">"ConfigurationUrlSasToken requires that configuration.url is specified"</span></span>

<span data-ttu-id="84514-244">"ConfigurationDataUrlSasToken vyžaduje, aby byl že tento configurationData.url je zadán"</span><span class="sxs-lookup"><span data-stu-id="84514-244">"ConfigurationDataUrlSasToken requires that configurationData.url is specified"</span></span>

<span data-ttu-id="84514-245">Problém: Definované vlastnosti potřebuje další vlastnost, která nebyla nalezena.</span><span class="sxs-lookup"><span data-stu-id="84514-245">Problem: A defined property needs another property that is missing.</span></span>

<span data-ttu-id="84514-246">Řešení:</span><span class="sxs-lookup"><span data-stu-id="84514-246">Solutions:</span></span> 

* <span data-ttu-id="84514-247">Zadejte chybějící vlastnost hello.</span><span class="sxs-lookup"><span data-stu-id="84514-247">Provide hello missing property.</span></span>
* <span data-ttu-id="84514-248">Odeberte vlastnost hello, který potřebuje hello chybějící vlastnost.</span><span class="sxs-lookup"><span data-stu-id="84514-248">Remove hello property that needs hello missing property.</span></span>

## <a name="next-steps"></a><span data-ttu-id="84514-249">Další kroky</span><span class="sxs-lookup"><span data-stu-id="84514-249">Next Steps</span></span>
<span data-ttu-id="84514-250">Další informace o DSC a sadách škálování virtuálních počítačů [použití sad škálování k virtuálnímu počítači s hello rozšíření DSC Azure](../../virtual-machine-scale-sets/virtual-machine-scale-sets-dsc.md)</span><span class="sxs-lookup"><span data-stu-id="84514-250">Learn about DSC and virtual machine scale sets in [Using Virtual Machine Scale Sets with hello Azure DSC Extension](../../virtual-machine-scale-sets/virtual-machine-scale-sets-dsc.md)</span></span>

<span data-ttu-id="84514-251">Najít další podrobnosti na [správu zabezpečení přihlašovacích údajů DSC](extensions-dsc-credentials.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="84514-251">Find more details on [DSC's secure credential management](extensions-dsc-credentials.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> 

<span data-ttu-id="84514-252">Další informace o hello Azure DSC rozšíření obslužné rutiny, najdete v části [Úvod toohello konfigurace požadovaného stavu Azure rozšíření obslužné rutiny](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="84514-252">For more information on hello Azure DSC extension handler, see [Introduction toohello Azure Desired State Configuration extension handler](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> 

<span data-ttu-id="84514-253">Další informace o DSC Powershellu [navštivte centru dokumentace prostředí PowerShell hello](https://msdn.microsoft.com/powershell/dsc/overview).</span><span class="sxs-lookup"><span data-stu-id="84514-253">For more information about PowerShell DSC, [visit hello PowerShell documentation center](https://msdn.microsoft.com/powershell/dsc/overview).</span></span> 

