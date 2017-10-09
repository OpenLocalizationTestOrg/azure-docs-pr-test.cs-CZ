### <a name="install-via-composer"></a><span data-ttu-id="3f83e-101">Nainstalovat prostřednictvím autora</span><span class="sxs-lookup"><span data-stu-id="3f83e-101">Install via Composer</span></span>
1. <span data-ttu-id="3f83e-102">[Nainstalovat Git][install-git].</span><span class="sxs-lookup"><span data-stu-id="3f83e-102">[Install Git][install-git].</span></span> <span data-ttu-id="3f83e-103">Všimněte si, že v systému Windows, musíte taky přidat proměnné prostředí PATH spustitelné tooyour Git hello.</span><span class="sxs-lookup"><span data-stu-id="3f83e-103">Note that on Windows, you must also add hello Git executable tooyour PATH environment variable.</span></span> 
2. <span data-ttu-id="3f83e-104">Vytvořte soubor s názvem **composer.json** v kořenu projektu hello a přidejte následující kód tooit hello:</span><span class="sxs-lookup"><span data-stu-id="3f83e-104">Create a file named **composer.json** in hello root of your project and add hello following code tooit:</span></span>
   
    ```json
    {
      "require": {
        "microsoft/windowsazure": "^0.4"
      }
    }
    ```
3. <span data-ttu-id="3f83e-105">Stáhněte si  **[composer.phar] [ composer-phar]**  v kořenového adresáře projektu.</span><span class="sxs-lookup"><span data-stu-id="3f83e-105">Download **[composer.phar][composer-phar]** in your project root.</span></span>
4. <span data-ttu-id="3f83e-106">Otevřete příkazový řádek a spusťte následující příkaz do kořenového adresáře projektu hello</span><span class="sxs-lookup"><span data-stu-id="3f83e-106">Open a command prompt and execute hello following command in your project root</span></span>
   
    ```
    php composer.phar install
    ```

[php-sdk-github]: http://go.microsoft.com/fwlink/?LinkId=252719
[install-git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[download-SDK-PHP]: ../articles/php-download-sdk.md
[composer-phar]: http://getcomposer.org/composer.phar
