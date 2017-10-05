---
title: "Povolení nebo zákaz monitorování virtuálních počítačů Azure"
description: "Popisuje, jak povolit nebo zakázat monitorování virtuálních počítačů Azure"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: kmouss
manager: timlt
editor: 
ms.assetid: 6ce366d2-bd4c-4fef-a8f5-a3ae2374abcc
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/08/2016
ms.author: kmouss
ms.openlocfilehash: 9b2fe579113d6ca6bfd27d82eb9d4657657d44ff
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="enable-or-disable-azure-vm-monitoring"></a><span data-ttu-id="a2ae9-103">Povolte nebo zakažte monitorování virtuálních počítačů Azure</span><span class="sxs-lookup"><span data-stu-id="a2ae9-103">Enable or Disable Azure VM Monitoring</span></span>

<span data-ttu-id="a2ae9-104">Tato část popisuje, jak povolit nebo zakázat monitorování na virtuální počítače běžící v Azure.</span><span class="sxs-lookup"><span data-stu-id="a2ae9-104">This section describes how to enable or disable monitoring on Virtual machines running on Azure.</span></span> <span data-ttu-id="a2ae9-105">Můžete povolit nebo zakázat monitorování pomocí portálu nebo rozhraní příkazového řádku Azure pro Mac, Linux a Windows (Azure CLI).</span><span class="sxs-lookup"><span data-stu-id="a2ae9-105">You can enable or disable monitoring using the portal or Azure Command-line Interface for Mac, Linux, and Windows (the Azure CLI).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a2ae9-106">Tento dokument popisuje 2.3 verzi rozšíření diagnostiky Linux, která se už nepoužívá.</span><span class="sxs-lookup"><span data-stu-id="a2ae9-106">This document describes version 2.3 of the Linux Diagnostic Extension, which has been deprecated.</span></span> <span data-ttu-id="a2ae9-107">Verze 2.3 bude až do 30. června 2018 podporována.</span><span class="sxs-lookup"><span data-stu-id="a2ae9-107">Version 2.3 will be supported until June 30, 2018.</span></span>
>
> <span data-ttu-id="a2ae9-108">Místo toho lze povolit rozšíření diagnostiky Linux verze 3.0.</span><span class="sxs-lookup"><span data-stu-id="a2ae9-108">Version 3.0 of the Linux Diagnostic Extension can be enabled instead.</span></span> <span data-ttu-id="a2ae9-109">Další informace najdete v tématu [dokumentaci](./diagnostic-extension.md).</span><span class="sxs-lookup"><span data-stu-id="a2ae9-109">For more information, see [the documentation](./diagnostic-extension.md).</span></span>

## <a name="enable--disable-monitoring-through-the-azure-portal"></a><span data-ttu-id="a2ae9-110">Povolit / zakázat monitorování prostřednictvím portálu Azure</span><span class="sxs-lookup"><span data-stu-id="a2ae9-110">Enable / Disable Monitoring through the Azure portal</span></span>

<span data-ttu-id="a2ae9-111">Můžete povolit monitorování vašeho virtuálního počítače Azure, který poskytuje data o vaší instance v období 1 minutu.</span><span class="sxs-lookup"><span data-stu-id="a2ae9-111">You can enable  monitoring of your Azure VM, which provides data about your instance in 1-minute periods.</span></span> <span data-ttu-id="a2ae9-112">(použít změny úložiště).</span><span class="sxs-lookup"><span data-stu-id="a2ae9-112">(storage changes apply).</span></span> <span data-ttu-id="a2ae9-113">Podrobné diagnostická data, pak je k dispozici pro virtuální počítač v portálu grafy nebo přes rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="a2ae9-113">Detailed diagnostics data is then available for the VM in the portal graphs or through the API.</span></span> <span data-ttu-id="a2ae9-114">Ve výchozím nastavení portál Azure umožňuje monitorování na základě hostitele omezenou sadu metriky.</span><span class="sxs-lookup"><span data-stu-id="a2ae9-114">By default, Azure portal enables host-based monitoring of a limited set of metrics.</span></span> <span data-ttu-id="a2ae9-115">Můžete povolit sledování metrik z v rámci virtuálního počítače spuštěného virtuálního počítače nebo v zastaveném stavu.</span><span class="sxs-lookup"><span data-stu-id="a2ae9-115">You can enable monitoring of metrics from within a VM while the VM is running or in stopped state.</span></span>

* <span data-ttu-id="a2ae9-116">Otevřete portál Azure na adrese [https://portal.azure.com](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="a2ae9-116">Open the Azure portal at [https://portal.azure.com](https://portal.azure.com).</span></span>
* <span data-ttu-id="a2ae9-117">V levém navigačním panelu klikněte na virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="a2ae9-117">In the left navigation, click Virtual machines.</span></span>
* <span data-ttu-id="a2ae9-118">V seznamu virtuálních počítačů vyberte instanci spuštěná nebo zastavená.</span><span class="sxs-lookup"><span data-stu-id="a2ae9-118">In the list Virtual machines, select a running or stopped instance.</span></span> <span data-ttu-id="a2ae9-119">Otevře se okno "Virtuální počítač".</span><span class="sxs-lookup"><span data-stu-id="a2ae9-119">The "Virtual machine" blade opens.</span></span>
* <span data-ttu-id="a2ae9-120">Klikněte na tlačítko všechna nastavení.</span><span class="sxs-lookup"><span data-stu-id="a2ae9-120">Click All settings.</span></span>
* <span data-ttu-id="a2ae9-121">Klikněte na tlačítko diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="a2ae9-121">Click Diagnostics.</span></span>
* <span data-ttu-id="a2ae9-122">Změnit stav zapnuto nebo vypnuto.</span><span class="sxs-lookup"><span data-stu-id="a2ae9-122">Change status to On or Off.</span></span> <span data-ttu-id="a2ae9-123">Můžete také vybrat v tomto okně úroveň monitorování podrobnosti, které chcete povolit pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="a2ae9-123">You can also pick in this blade the level of monitoring details you would like to enable for your virtual machine.</span></span>

![Povolit nebo zakázat monitorování prostřednictvím portálu Azure.][1]

## <a name="enable--disable-monitoring-with-azure-cli"></a><span data-ttu-id="a2ae9-125">Povolit / zakázat monitorování pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="a2ae9-125">Enable / Disable Monitoring with Azure CLI</span></span>

<span data-ttu-id="a2ae9-126">Chcete-li povolit monitorování pro virtuální počítač Azure.</span><span class="sxs-lookup"><span data-stu-id="a2ae9-126">To enable monitoring for an Azure VM.</span></span>

* <span data-ttu-id="a2ae9-127">Vytvořte soubor (s názvem například PrivateConfig.json):</span><span class="sxs-lookup"><span data-stu-id="a2ae9-127">Create a file (named such as PrivateConfig.json):</span></span>

```json
{
        "storageAccountName":"the storage account to receive data",
        "storageAccountKey":"the key of the account"
}
```

* <span data-ttu-id="a2ae9-128">Povolte rozšíření prostřednictvím rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="a2ae9-128">Enable the extension via Azure CLI.</span></span>

```azurecli
azure vm extension set myvm LinuxDiagnostic Microsoft.OSTCExtensions 2.3 --private-config-path PrivateConfig.json
```

<span data-ttu-id="a2ae9-129">Další informace najdete v tématu [pomocí Linux diagnostiky rozšíření výkonu monitorování Linux Virtuálního počítače a diagnostických dat](classic/diagnostic-extension-v2.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="a2ae9-129">For more information, see [Using Linux Diagnostic Extension to Monitor Linux VM’s performance and diagnostic data](classic/diagnostic-extension-v2.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span></span>

<!--Image references-->
[1]: ./media/vm-monitoring/portal-enable-disable.png
