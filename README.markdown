Nette Translator (c) Patrik Votoček (Vrtak-CZ), 2010 (http://patrik.votocek.cz)


Note
========
This is short manual how to use Nette Translator in the newest Nette 2.0 in its most simple version.
No need to edit or operate with .po/.mo files required.

Actual info and manual: http://wiki.nette.org/cs/cookbook/zprovozneni-prekladace-nettetranslator

config.neon
----
    netteTranslator:
        lang: cs
        files:
            front: %appDir%/lang

Bootstrap.php
----
    // this is new
    $configurator->onCompile[] = function ($configurator, $compiler) {
        $compiler->addExtension('netteTranslator', new NetteTranslator\NetteTranslatorExtension);
    };
    
    // put new lines the following line
    $container = $configurator->createContainer();

BasePresenter.php
----
Basic usage + language change

    /** @persistent */
    public $lang;
    
    /** @var NetteTranslator\Gettext */
    protected $translator;
    
    
    /**
     * Inject translator
     * @param NetteTranslator\Gettext
     */
    public function injectTranslator(NetteTranslator\Gettext $translator)
    {
        $this->translator = $translator;
    }


    public function createTemplate($class = NULL)
    {
    	$template = parent::createTemplate($class);
    
    	// pokud není nastaven, použijeme defaultní z configu
    	if (!isset($this->lang)) {
    		$this->lang = $this->translator->getLang();
    	}
    
    	$this->translator->setLang($this->lang); // nastavíme jazyk
    	$template->setTranslator($this->translator);
    
    	return $template;
    }
