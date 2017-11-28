<span data-ttu-id="eb5e6-101">Azure určí verzi jazyka Python, která se má použít pro virtuální prostředí, pomocí tohoto pořadí priorit:</span><span class="sxs-lookup"><span data-stu-id="eb5e6-101">Azure will determine the version of Python to use for its virtual environment with the following priority:</span></span>

1. <span data-ttu-id="eb5e6-102">Verze zadaná v souboru runtime.txt v kořenové složce</span><span class="sxs-lookup"><span data-stu-id="eb5e6-102">version specified in runtime.txt in the root folder</span></span>
2. <span data-ttu-id="eb5e6-103">Verze zadaná v nastavení jazyka Python v konfiguraci webové aplikace (okno **Nastavení** > **Nastavení aplikace** pro danou webovou aplikaci na webu Azure Portal)</span><span class="sxs-lookup"><span data-stu-id="eb5e6-103">version specified by Python setting in the web app configuration (the **Settings** > **Application Settings** blade for your web app in the Azure Portal)</span></span>
3. <span data-ttu-id="eb5e6-104">Pokud není uvedená žádná z výše uvedených verzí, použije se výchozí verze python-2.7</span><span class="sxs-lookup"><span data-stu-id="eb5e6-104">python-2.7 is the default if none of the above are specified</span></span>

<span data-ttu-id="eb5e6-105">Platné hodnoty obsahu souboru</span><span class="sxs-lookup"><span data-stu-id="eb5e6-105">Valid values for the contents of</span></span> 

    \runtime.txt

<span data-ttu-id="eb5e6-106">jsou následující:</span><span class="sxs-lookup"><span data-stu-id="eb5e6-106">are:</span></span>

* <span data-ttu-id="eb5e6-107">python-2.7</span><span class="sxs-lookup"><span data-stu-id="eb5e6-107">python-2.7</span></span>
* <span data-ttu-id="eb5e6-108">python-3.4</span><span class="sxs-lookup"><span data-stu-id="eb5e6-108">python-3.4</span></span>

<span data-ttu-id="eb5e6-109">Pokud je uvedená mikroverze (třetí číslice), bude se ignorovat.</span><span class="sxs-lookup"><span data-stu-id="eb5e6-109">If the micro version (third digit) is specified, it is ignored.</span></span>

