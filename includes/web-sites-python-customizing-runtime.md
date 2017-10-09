<span data-ttu-id="23964-101">Azure určí hello verzi toouse Python pro jeho virtuální prostředí s hello následující priority:</span><span class="sxs-lookup"><span data-stu-id="23964-101">Azure will determine hello version of Python toouse for its virtual environment with hello following priority:</span></span>

1. <span data-ttu-id="23964-102">verze zadaná v souboru runtime.txt v kořenové složce hello</span><span class="sxs-lookup"><span data-stu-id="23964-102">version specified in runtime.txt in hello root folder</span></span>
2. <span data-ttu-id="23964-103">verze zadaná v nastavení jazyka Python v konfiguraci webové aplikace hello (hello **nastavení** > **nastavení aplikace** okno pro vaši webovou aplikaci v hello portál Azure)</span><span class="sxs-lookup"><span data-stu-id="23964-103">version specified by Python setting in hello web app configuration (hello **Settings** > **Application Settings** blade for your web app in hello Azure Portal)</span></span>
3. <span data-ttu-id="23964-104">Python 2.7 je výchozí hello, pokud nejsou zadány žádné z výše uvedených hello</span><span class="sxs-lookup"><span data-stu-id="23964-104">python-2.7 is hello default if none of hello above are specified</span></span>

<span data-ttu-id="23964-105">Platné hodnoty pro obsah hello</span><span class="sxs-lookup"><span data-stu-id="23964-105">Valid values for hello contents of</span></span> 

    \runtime.txt

<span data-ttu-id="23964-106">jsou následující:</span><span class="sxs-lookup"><span data-stu-id="23964-106">are:</span></span>

* <span data-ttu-id="23964-107">python-2.7</span><span class="sxs-lookup"><span data-stu-id="23964-107">python-2.7</span></span>
* <span data-ttu-id="23964-108">python-3.4</span><span class="sxs-lookup"><span data-stu-id="23964-108">python-3.4</span></span>

<span data-ttu-id="23964-109">Pokud hello mikroverze (třetí číslice) je zadán, bude se ignorovat.</span><span class="sxs-lookup"><span data-stu-id="23964-109">If hello micro version (third digit) is specified, it is ignored.</span></span>

