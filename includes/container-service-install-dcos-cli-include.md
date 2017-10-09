---
title: "aaaInstall hello rozhraní příkazového řádku DC/OS | Microsoft Docs"
description: "Nainstalujte hello rozhraní příkazového řádku DC/OS."
services: container-service
documentationcenter: 
author: rgardler
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Kontejnery, mikroslužby, DC/OS, Azure"
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/10/2016
ms.author: rogardle
ms.openlocfilehash: b077c05beff9a5638486ea5efe9df31089e32701
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
> [!NOTE]
> <span data-ttu-id="5e1cb-104">To slouží k práci s clustery ACS založenými na DC/OS.</span><span class="sxs-lookup"><span data-stu-id="5e1cb-104">This is for working with DC/OS-based ACS clusters.</span></span> <span data-ttu-id="5e1cb-105">Neexistuje žádné toodo nutné to pro clustery ACS založené na metodě Swarm.</span><span class="sxs-lookup"><span data-stu-id="5e1cb-105">There is no need toodo this for Swarm-based ACS clusters.</span></span>
> 
> 

<span data-ttu-id="5e1cb-106">První, [připojení clusteru tooyour ACS založenými na DC/OS](../articles/container-service/container-service-connect.md).</span><span class="sxs-lookup"><span data-stu-id="5e1cb-106">First, [connect tooyour DC/OS-based ACS cluster](../articles/container-service/container-service-connect.md).</span></span> <span data-ttu-id="5e1cb-107">Jakmile to uděláte, můžete nainstalovat hello rozhraní příkazového řádku DC/OS na klientský počítač s příkazy hello níže:</span><span class="sxs-lookup"><span data-stu-id="5e1cb-107">Once you have done this, you can install hello DC/OS CLI on your client machine with hello commands below:</span></span>

```bash
sudo pip install virtualenv
mkdir dcos && cd dcos
wget https://raw.githubusercontent.com/mesosphere/dcos-cli/master/bin/install/install-optout-dcos-cli.sh
chmod +x install-optout-dcos-cli.sh
./install-optout-dcos-cli.sh . http://localhost --add-path yes
```

<span data-ttu-id="5e1cb-108">Pokud používáte starší verzi Pythonu, můžou se zobrazovat upozornění „InsecurePlatformWarning“.</span><span class="sxs-lookup"><span data-stu-id="5e1cb-108">If you are using an old version of Python, you may notice some "InsecurePlatformWarnings".</span></span> <span data-ttu-id="5e1cb-109">Ta můžete bezpečně ignorovat.</span><span class="sxs-lookup"><span data-stu-id="5e1cb-109">You can safely ignore these.</span></span>

<span data-ttu-id="5e1cb-110">V pořadí tooget byla spuštěna bez restartovat své prostředí spusťte příkaz:</span><span class="sxs-lookup"><span data-stu-id="5e1cb-110">In order tooget started without restarting your shell, run:</span></span>

```bash
source ~/.bashrc
```

<span data-ttu-id="5e1cb-111">Pokud spustíte nové prostředí, nebude tento krok nutný.</span><span class="sxs-lookup"><span data-stu-id="5e1cb-111">This step will not be necessary when you start new shells.</span></span>

<span data-ttu-id="5e1cb-112">Teď si můžete ověřit, že hello je nainstalované rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="5e1cb-112">Now you can confirm that hello CLI is installed:</span></span>

```bash
dcos --help
```