---
title: "aaaInstall a konfigurace virtuálních počítačů tooprovision Terraform a další infrastrukturou v Azure | Microsoft Docs"
description: "Zjistěte, jak tooinstall a nakonfigurujte Terraform toocreate Azure prostředky"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: echuvyrov
manager: jtalkar
editor: na
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/14/2017
ms.author: echuvyrov
ms.openlocfilehash: b6706f53b21347442def05a592c30a2d22718984
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="install-and-configure-terraform-tooprovision-vms-and-other-infrastructure-into-azure"></a>Instalace a konfigurace virtuálních počítačů tooprovision Terraform a další infrastrukturou do Azure 
Tento článek popisuje hello potřebné kroky tooinstall a nakonfigurovat Terraform tooprovision prostředky, jako jsou virtuální počítače do Azure. Se dozvíte, jak toocreate a používání Azure pověření tooenable Terraform tooprovision cloudovým prostředkům zabezpečené způsobem.

HashiCorp Terraform poskytuje snadný způsob toodefine a nasazení infrastruktury cloudu pomocí vlastní ukázka jazyk, kterému se říká HashiCorp konfigurace jazyk (kompatibilního hardwaru). Tato vlastní jazyk je [snadno toowrite a snadno toounderstand](terraform-create-complete-vm.md). Kromě toho pomocí hello `terraform plan` příkaz hello změny tooyour infrastruktury můžete vizualizovat, než je potvrzení. Postupujte podle těchto kroků toostart Terraform pomocí Azure.

## <a name="install-terraform"></a>Nainstalujte Terraform
tooinstall Terraform, [Stáhnout](https://www.terraform.io/downloads.html) hello balíčku pro váš operační systém do samostatné instalační adresář. stažení Hello obsahuje jeden spustitelný soubor, pro které byste měli také definovat globální cestu. Pokyny, jak tooset hello cestu na Linuxu a Macu, naleznete příliš[tato webová stránka](https://stackoverflow.com/questions/14637979/how-to-permanently-set-path-on-linux). Pokyny, jak tooset hello cesty v systému Windows, naleznete příliš[tato webová stránka](https://stackoverflow.com/questions/1618280/where-can-i-set-path-to-make-exe-on-windows). tooverify instalace systému, spustit hello `terraform` příkaz. Zobrazí seznam dostupných možností Terraform jako výstup.

Dále musíte tooallow Terraform přístup tooyour předplatného Azure tooperform infrastruktury zřizování.

## <a name="set-up-terraform-access-tooazure"></a>Nastavení přístupu tooAzure Terraform
tooenable Terraform tooprovision prostředky do Azure, je nutné toocreate dvě entity ve službě Azure Active Directory (Azure AD): aplikace Azure AD a instanční objekt služby Azure AD. Poté použít tyto entity identifikátory v Terraform skripty. Hlavní název služby je místní instanci globální Azure AD aplikace. Objekt služby umožňuje granulární místní přístup k řízení tooglobal prostředkům.

Existuje několik způsobů toocreate aplikaci Azure AD a instanční objekt služby Azure AD. Hello nejjednodušší a nejrychlejší způsob v současné době jsou toouse Azure CLI 2.0, který [můžete stáhnout a nainstalovat na Windows, Linuxu nebo Macu](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli). Můžete taky použít PowerShell nebo Azure CLI 1.0 toocreate hello potřebná zabezpečovací infrastruktury. pokyny Hello ukazují, jak tooconfigure Terraform pro Azure. všechny tyto přístupy.

### <a name="use-azure-cli-20-for-windows-linux-or-mac-users"></a>Použití Azure CLI 2.0 (pro Windows, Linux nebo Mac. uživatelů) 
Po stažení a instalaci hello [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli), přihlaste se tooadminister vašeho předplatného Azure vydáním hello následující příkaz:

```
az login
```

>[!NOTE]
>Pokud používáte hello Čína, Azure v Německu nebo cloudů Azure Government, musíte toofirst hello rozhraní příkazového řádku Azure toowork nakonfigurovat příslušný cloud. Můžete provést spuštěním následující hello:

```
az cloud set --name AzureChinaCloud|AzureGermanCloud|AzureUSGovernment
```

Pokud máte víc předplatných Azure, jsou jejich podrobnosti vrácený hello `az login` příkaz. Sada hello `SUBSCRIPTION_ID` vrácena hodnota hello toohold proměnné prostředí hello `id` pole z předplatného hello chcete toouse. 

Nastavte předplatné hello chcete toouse pro tuto relaci.

```
az account set --subscription="${SUBSCRIPTION_ID}"
```

Dotaz na ID předplatného hello účtu tooget hello a hodnoty ID klienta.

```
az account show --query "{subscriptionId:id, tenantId:tenantId}"
```

Dále vytvořte samostatné přihlašovací údaje pro Terraform.

```
az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/${SUBSCRIPTION_ID}"
```

AppId, heslo, sp_name a klienta se vrátí. Poznamenejte si hello appId a heslo.

tooconfirm přihlašovacích údajů (instanční objekt), otevřete nové prostředí a spusťte následující příkazy hello. SUBSTITUTE hello vrátil hodnoty sp_name, heslo a klienta:

```
az login --service-principal -u SP_NAME -p PASSWORD --tenant TENANT
az vm list-sizes --location westus
```

### <a name="use-powershell-for-windows-users"></a>Pomocí prostředí PowerShell (pro uživatele systému Windows) 
toouse Windows počítač toowrite a spuštění vaší Terraform skripty a toouse prostředí PowerShell pro úlohy konfigurace, nakonfigurovat svůj počítač má prostředí PowerShell nástroje hello. 

1. Instalace nástrojů pro prostředí PowerShell pomocí následujících kroků hello v [nainstalovat a nakonfigurovat Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/install-azurerm-ps). 

2. Stažení a spuštění hello [skriptu azure setup.ps1](https://github.com/echuvyrov/terraform101/blob/master/azureSetup.ps1) z konzoly Powershellu hello.

3. toorun hello azure setup.ps1 skriptu, jeho stažení a spuštění hello `./azure-setup.ps1 setup` v konzole PowerShell hello příkaz. Potom se přihlaste tooyour předplatné s oprávněními správce.

4. Zadejte název aplikace (libovolný řetězec, vyžaduje) po zobrazení výzvy. Volitelně můžete zadejte silné heslo po zobrazení výzvy. Pokud nezadáte heslo, silné heslo se generuje pro můžete pomocí knihovny .NET zabezpečení.

### <a name="use-azure-cli-10-for-linux-or-mac-users"></a>Použití Azure CLI 1.0 (pro Linux nebo Mac. uživatelů)
tooget začít s Terraform na počítače se systémem Linux nebo Mac s Azure CLI verze 1.0, instalace hello správné knihovny v počítači.  

1. Instalace nástrojů příkazového řádku Azure xPlat podle následujících kroků hello v [nainstalovat Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli). 

2. Stáhněte a nainstalujte podle pokynů hello v procesoru JSON [stáhnout jq](https://stedolan.github.io/jq/download/).

3. Stažení a spuštění hello [skriptu azure setup.sh](https://github.com/mitchellh/packer/blob/master/contrib/azure-setup.sh) bash skriptu z konzoly hello.

4. toorun hello azure setup.sh skriptu, jeho stažení a spuštění hello `./azure-setup setup` příkazu z konzoly hello. Potom se přihlaste tooyour předplatné s oprávněními správce.
 
5. Zadejte název aplikace (libovolný řetězec, vyžaduje) po zobrazení výzvy. Volitelně můžete zadejte silné heslo po zobrazení výzvy. Pokud nezadáte heslo, silné heslo se generuje pro můžete pomocí knihovny .NET zabezpečení.

Všechny předchozí skripty hello vytvoření aplikace Azure AD a služby hlavní. Hello instanční objekt získá Přispěvatel nebo vlastníka úroveň přístupu na základě předplatného hello. Z důvodu hello vysokou úroveň udělí přístup byste měli vždy chránit informace o zabezpečení hello generované tyto skripty. Poznamenejte si všechny čtyři kusy informace o zabezpečení poskytované tyto skripty: appId, heslo, ID_ODBĚRU a tenant_id.

## <a name="set-environment-variables"></a>Proměnné prostředí sady
Po vytvoření a konfigurace objektu služby Azure AD, je nutné toolet Terraform vědět hello ID klienta, ID předplatného, ID klienta a tajný toouse klienta. Můžete to provést vložením tyto hodnoty v Terraform skripty, jak je popsáno v [vytvořit základní infrastruktura pomocí Terraform](terraform-create-complete-vm.md). Alternativně můžete nastavit hello následující proměnné prostředí (a tedy vyhnout se omylem přihlašuje nebo sdílení přihlašovacích údajů):

- ARM_SUBSCRIPTION_ID
- ARM_CLIENT_ID
- ARM_CLIENT_SECRET
- ARM_TENANT_ID

Tato ukázka prostředí skriptu tooset můžete použít tyto proměnné:

```
#!/bin/sh
echo "Setting environment variables for Terraform"
export ARM_SUBSCRIPTION_ID=your_subscription_id
export ARM_CLIENT_ID=your_appId
export ARM_CLIENT_SECRET=your_password
export ARM_TENANT_ID=your_tenant_id
```

Kromě toho pokud použijete Terraform s Azure v Číně nebo buď Azure Government nebo Azure v Německu, je třeba proměnnou prostředí hello tooset správně.

## <a name="next-steps"></a>Další kroky
Teď už máte nainstalovaný Terraform a nakonfigurovat přihlašovací údaje Azure, takže můžete začít nasazovat infrastruktury do vašeho předplatného Azure. Dále se naučíte, jak příliš[vytvoření infrastruktury s Terraform](terraform-create-complete-vm.md).
