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
# <a name="windows-vmss-and-desired-state-configuration-with-azure-resource-manager-templates"></a>Windows VMSS a konfigurace požadovaného stavu pomocí šablon Azure Resource Manageru
Tento článek popisuje hello šablony Resource Manageru pro hello [obslužná rutina rozšíření konfigurace požadovaného stavu](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

## <a name="template-example-for-a-windows-vm"></a>Příklad šablony pro virtuální počítač s Windows
Hello následující fragment kódu přejde do hello oddíl hello šablony.

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

## <a name="template-example-for-windows-vmss"></a>Příklad šablony Windows VMSS
Oddíl "vlastnosti" hello "VirtualMachineProfile", "extensionProfile" atribut má VMSS uzel. V části "rozšíření" se přidá DSC. 

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

## <a name="detailed-settings-information"></a>Informace o podrobné nastavení
Hello následující schéma je pro část nastavení hello hello rozšíření Azure DSC v šablonu Azure Resource Manager.

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

## <a name="details"></a>Podrobnosti
| Název vlastnosti | Typ | Popis |
| --- | --- | --- |
| settings.wmfVersion |Řetězec |Určuje verzi hello hello Windows Management Framework, který by měly být nainstalovány na vašem virtuálním počítači. Nastavení této vlastnosti too'latest se nainstaluje hello nejaktuálnější verzi WMF. Hello pouze aktuální možné hodnoty pro tuto vlastnost jsou **'4.0', '5.0', ' 5.0PP' a 'nejnovější'**. Tyto hodnoty jsou tooupdates subjektu. Hello výchozí hodnota je 'nejnovější'. |
| Settings.Configuration.URL |Řetězec |Určuje umístění hello adresu URL, ze které toodownload souboru zip konfigurace DSC. Pokud zadaná adresa URL hello vyžaduje SAS token pro přístup, je třeba tooset hello protectedSettings.configurationUrlSasToken toohello hodnota vlastnosti tokenu SAS. Tato vlastnost je vyžadována, pokud jsou definovány settings.configuration.script nebo settings.configuration.function. |
| Settings.Configuration.Script |Řetězec |Určuje název souboru hello hello skriptu, který obsahuje hello definici konfigurace DSC. Tento skript musí být v kořenové složce hello hello zip soubor stažený z adresy URL hello určený vlastností configuration.url hello. Tato vlastnost je vyžadována, pokud jsou definovány settings.configuration.url nebo settings.configuration.script. |
| Settings.Configuration.Function |Řetězec |Určuje název hello konfigurace DSC. Hello konfigurace s názvem musí být součástí skriptu hello definované configuration.script. Tato vlastnost je vyžadována, pokud jsou definovány settings.configuration.url nebo settings.configuration.function. |
| settings.configurationArguments |Kolekce |Definuje žádné parametry, které byste chtěli konfigurace DSC tooyour toopass. Tato vlastnost není zašifrován. |
| settings.configurationData.url |Řetězec |Určuje adresu URL hello z které toodownload svá konfigurační data (.psd1) souboru toouse jako vstup pro konfiguraci DSC. Pokud zadaná adresa URL hello vyžaduje SAS token pro přístup, je třeba tooset hello protectedSettings.configurationDataUrlSasToken toohello hodnota vlastnosti tokenu SAS. |
| settings.privacy.dataEnabled |Řetězec |Povolí nebo zakáže telemetrie kolekce. Hello pouze možné hodnoty pro tuto vlastnost jsou **povolit, "Zakázat", ", nebo $null**. Umožňuje telemetrie, a tato vlastnost prázdná nebo mít hodnotu null. Hello výchozí hodnota je ". [Další informace](https://blogs.msdn.microsoft.com/powershell/2016/02/02/azure-dsc-extension-data-collection-2/) |
| settings.advancedOptions.downloadMappings |Kolekce |Určuje alternativní umístění, ze které toodownload hello WMF. [Další informace](http://blogs.msdn.com/b/powershell/archive/2015/10/21/azure-dsc-extension-2-2-amp-how-to-map-downloads-of-the-extension-dependencies-to-your-own-location.aspx) |
| protectedSettings.configurationArguments |Kolekce |Definuje žádné parametry, které byste chtěli konfigurace DSC tooyour toopass. Tato vlastnost je zašifrován. |
| protectedSettings.configurationUrlSasToken |Řetězec |Určuje hello SAS token tooaccess hello URL definované configuration.url. Tato vlastnost je zašifrován. |
| protectedSettings.configurationDataUrlSasToken |Řetězec |Určuje hello SAS token tooaccess hello URL definované configurationData.url. Tato vlastnost je zašifrován. |

## <a name="settings-vs-protectedsettings"></a>Nastavení vs. ProtectedSettings
Všechna nastavení se ukládají do textového souboru nastavení na hello virtuálních počítačů.
Vlastnosti v části "nastavení" jsou veřejné vlastnosti, protože nejsou šifrovány hello nastavení textového souboru.
Vlastnosti v části 'protectedSettings' jsou šifrované pomocí certifikátu a nejsou zobrazeny ve formátu prostého textu v tomto souboru na hello virtuálních počítačů.

Pokud konfigurace hello potřebuje přihlašovací údaje, můžou být součástí protectedSettings:

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

## <a name="example"></a>Příklad
Hello následující příklad je odvozena z hello oddílu hello "Začínáme" [stránku přehled obslužná rutina rozšíření DSC](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
Tento příklad používá šablony Resource Manager místo rutiny toodeploy hello rozšíření. Uložte konfiguraci "IisInstall.ps1" hello, jeho umístění. ZIP – soubory a nahrávání hello soubor v přístupné adresy URL. Tento příklad používá úložiště objektů blob v Azure, ale je možné toodownload. Soubory ZIP libovolný odkudkoli.

V šabloně Azure Resource Manager hello hello následující kód dá pokyn hello virtuálních počítačů toodownload hello správný soubor a spusťte hello odpovídající funkce prostředí PowerShell:

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

## <a name="updating-from-hello-previous-format"></a>Aktualizace z hello předchozí formátu
Všechna nastavení v hello předchozí formátu (obsahující hello veřejné vlastnosti ModulesUrl ConfigurationFunction, SasToken nebo vlastnosti) automaticky přizpůsobit toohello současném formátu a spusťte stejně jako před.

Následující schéma Hello je co hello předchozí nastavení schématu hledá jako:

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

Zde je, jak předchozí formátu hello přizpůsobuje toohello aktuální formát:

| Název vlastnosti | Předchozí ekvivalent schématu |
| --- | --- |
| settings.wmfVersion |nastavení. WMFVersion |
| Settings.Configuration.URL |nastavení. ModulesUrl |
| Settings.Configuration.Script |První část nastavení. ConfigurationFunction (před '\\\\.) |
| Settings.Configuration.Function |Druhá část nastavení. ConfigurationFunction (po '\\\\.) |
| settings.configurationArguments |nastavení. Vlastnosti |
| settings.configurationData.url |protectedSettings.DataBlobUri (bez tokenu SAS) |
| settings.privacy.dataEnabled |nastavení. Privacy.DataEnabled |
| settings.advancedOptions.downloadMappings |nastavení. AdvancedOptions.DownloadMappings |
| protectedSettings.configurationArguments |protectedSettings.Properties |
| protectedSettings.configurationUrlSasToken |nastavení. SasToken |
| protectedSettings.configurationDataUrlSasToken |Token SAS z protectedSettings.DataBlobUri |

## <a name="troubleshooting---error-code-1100"></a>Řešení potíží – kód chyby 1100
Kód chyby 1100 označuje, že došlo k potížím s hello rozšíření toohello DSC vstup uživatele.
Tyto chyby textu Hello je proměnná a může se změnit.
Zde jsou některé hello chyb, které může spustit do a jak můžete opravit je.

### <a name="invalid-values"></a>Neplatné hodnoty
"Je Privacy.dataCollection: {0}. Hello pouze možné hodnoty jsou ","Zapnout"a"Zakázat"" "je WmfVersion: {0}. Pouze možné hodnoty jsou... a 'nejnovější' "

Problém: Zadaná hodnota není povolena.

Řešení: Změňte hello neplatná hodnota tooa platnou hodnotu. Najdete hello tabulce v části Podrobnosti o hello.

### <a name="invalid-url"></a>Neplatná adresa URL
"Je ConfigurationData.url: {0}. Toto není platná adresa URL.""je DataBlobUri: {0}. Toto není platná adresa URL.""je Configuration.url: {0}. Toto není platná adresa URL."

Problém: A, pokud že adresa URL není platná.

Řešení: Zkontrolujte všechny zadané URL. Ujistěte se, že všechny adresy URL přeložit umístění toovalid, kterým rozšíření hello přístup na vzdáleném počítači hello.

### <a name="invalid-configurationargument-type"></a>Neplatný typ ConfigurationArgument
"Neplatný configurationArguments typu {0}"

Problém: hello ConfigurationArguments vlastnost nelze vyřešit tooa zatřiďovací tabulky objektu. 

Řešení: Zkontrolujte vaši ConfigurationArguments vlastnost zatřiďovací tabulku. Použijte formát hello uvedené v předcházející příklad hello. Sledujte uvozovky, čárky a složené závorky.

### <a name="duplicate-configurationarguments"></a>Duplicitní ConfigurationArguments
"Nalezena ve veřejných i chráněný configurationArguments duplicitní argumenty {0}."

Problém: hello ConfigurationArguments v nastavení veřejných a hello ConfigurationArguments v chráněných nastavení obsahovat vlastnosti s hello stejný název.

Řešení: Odeberte jedno z hello duplicitní vlastnosti.

### <a name="missing-properties"></a>Chybí vlastnosti
"Configuration.function vyžaduje, aby byl zadán configuration.url nebo configuration.module"

"Configuration.url vyžaduje, aby byl že tento configuration.script je zadán"

"Configuration.script vyžaduje, aby byl že tento configuration.url je zadán"

"Configuration.url vyžaduje, aby byl že tento configuration.function je zadán"

"ConfigurationUrlSasToken vyžaduje, aby byl že tento configuration.url je zadán"

"ConfigurationDataUrlSasToken vyžaduje, aby byl že tento configurationData.url je zadán"

Problém: Definované vlastnosti potřebuje další vlastnost, která nebyla nalezena.

Řešení: 

* Zadejte chybějící vlastnost hello.
* Odeberte vlastnost hello, který potřebuje hello chybějící vlastnost.

## <a name="next-steps"></a>Další kroky
Další informace o DSC a sadách škálování virtuálních počítačů [použití sad škálování k virtuálnímu počítači s hello rozšíření DSC Azure](../../virtual-machine-scale-sets/virtual-machine-scale-sets-dsc.md)

Najít další podrobnosti na [správu zabezpečení přihlašovacích údajů DSC](extensions-dsc-credentials.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

Další informace o hello Azure DSC rozšíření obslužné rutiny, najdete v části [Úvod toohello konfigurace požadovaného stavu Azure rozšíření obslužné rutiny](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

Další informace o DSC Powershellu [navštivte centru dokumentace prostředí PowerShell hello](https://msdn.microsoft.com/powershell/dsc/overview). 

