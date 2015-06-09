.. _in-depth-guide.first-module:

Iniciando nosso primeiro Modulo "Blog"
======================================

Agora que você sabe o básico de uma aplicação esqueleto com Zend Framework 2, vamos continuar e criar nosso próprio
modulo. No vamosr criar um modulo chamado "Blog". Esse modulo irá mostrar uma lista de entradas no banco de dados que
representam um único artigo no blog. Cada artigo terá três propriedades: ``id``, ``text`` e ``title``. Nos vamos criar
formulários para cadastrar novos artigos no nosso banco de dados e editar os artigos existentes. Além disso nos vamos
fazer isso usando melhores práticas por todo o guia.

Escrevendo um novo Modulo
=========================

Vamos começar criando um novo diretório na pasta ``/module`` chamado ``Blog``.

Para ser reconhecido como Modulo pelo :ref:`ModuleManager <zend.module-manager-intro>`
tudo que precisamos fazer é criar uma classe PHP chamada ``Module`` dentro do nosso namespace do modulo, que é ``Blog``.
Então crie o arquivo ``/module/Blog/Module.php``

.. code-block:: php
   :linenos:

    <?php
    // Filename: /module/Blog/Module.php
    namespace Blog;

    class Module
    {
    }

Agora nos temos um modulo que pode ser detectado pelo :ref:`ModuleManager <zend.module-manager-intro>` do ZF2.
Vamos adcionar esse modulo a nossa aplicação. Mesmo que nosso modulo ainda não faça nada, tendo somente a classe
``Module.php`` permite que esse seja carregado pelo :ref:`ModuleManager <zend.module-manager-intro>` do ZF2.
Para fazer isso, inclua uma entrada para ``Blog`` no array de modulos dentro do arquivo principal de configuração em
``/config/application.config.php``:

.. code-block:: php
   :linenos:
   :emphasize-lines: 6

    <?php
    // Filename: /config/application.config.php
    return array(
        'modules' => array(
            'Application',
            'Blog'
        ),

        // ...
    );

Se você recarregar sua aplicação você não deve ver nenhuma mudança (mas também nenhum erro).

Nesse ponto é valido dar um passo atras para discutir para que servem os modulos. Em resumo, um modulo é um conjunto
de funcionalidades encapsuladas da sua aplicação. Um modulo pode incluir funcionalidades na aplicação que você pode ver
como o modulo de Blog; ou pode prover funcionalidades de segundo plano para outros modulos da aplicação usarem, como
interagir com uma API de terceiros.

Organizando seu código em modulos torna mais fácil a reutilização de funcionalidades em outras aplicações, ou o uso de
módulos escritos pela comunidade.

Configurando o Modulo
=====================

A próxima coisa que iremos fazer é adicionar uma rota na nossa aplicação para que nosso módulo possa ser acessado
pela URL ``localhost:8080/blog``. Nos fazemos isso adicionando as configurações de roteamento no nosso modulo, mas
antes nos precisamos deixar o ``ModuleManager`` saber que nosso modulo tem configurações que precisam ser carregadas.

Para isso precisamos adicionar um método ``getConfig()`` na classe ``Module`` que retorna a configuração. (Esse método é
definido na ``ConfigProviderInterface`` entretanto implementar essa interface de fato no modulo é opcional).
Esse método deve retirnar um ``array`` ou um objeto ``Traversable``. Continue editando seu ``/module/Blog/Module.php``:

.. code-block:: php
   :linenos:
   :emphasize-lines: 5,7,9-12

    <?php
    // Filename: /module/Blog/Module.php
    namespace Blog;

    use Zend\ModuleManager\Feature\ConfigProviderInterface;

    class Module implements ConfigProviderInterface
    {
        public function getConfig()
        {
            return array();
        }
    }

Com isso nosso modelo está agora apto a ser configurado. Arquivos de configuração podem se tornar muito grandes e manter
tudo dentro do método ``getConfig()`` não seria recomendado. Para ajudar a manter nosso projeto organizado nos vamos
colocar nosso array de configuração em um arquivo separado. Siga em frente e crie esse arquivo em
``/module/Blog/config/module.config.php``:

.. code-block:: php
   :linenos:

    <?php
    // Filename: /module/Blog/config/module.config.php
    return array();

Agora nos vamos reescrever o mmétodo ``getConfig()`` para incluir nosso mais recente arquivo ao inves de retornar
diretamente o array.

.. code-block:: php
   :linenos:
   :emphasize-lines: 11

    <?php
    // Filename: /module/Blog/Module.php
    namespace Blog;

    use Zend\ModuleManager\Feature\ConfigProviderInterface;

    class Module implements ConfigProviderInterface
    {
        public function getConfig()
        {
            return include __DIR__ . '/config/module.config.php';
        }
    }

Recarregue sua aplicação e você verá que tudo permanece como estava. Em seguida nos vamos adicionar a nova rota ao nosso
arquivo de configuração:

.. code-block:: php
   :linenos:
   :emphasize-lines: 9,11,15,18-19

    <?php
    // Filename: /module/Blog/config/module.config.php
    return array(
        // This lines opens the configuration for the RouteManager
        'router' => array(
            // Open configuration for all possible routes
            'routes' => array(
                // Define a new route called "post"
                'post' => array(
                    // Define the routes type to be "Zend\Mvc\Router\Http\Literal", which is basically just a string
                    'type' => 'literal',
                    // Configure the route itself
                    'options' => array(
                        // Listen to "/blog" as uri
                        'route'    => '/blog',
                        // Define default controller and action to be called when this route is matched
                        'defaults' => array(
                            'controller' => 'Blog\Controller\List',
                            'action'     => 'index',
                        )
                    )
                )
            )
        )
    );

Nos criamos agora uma rota chamada ``blog`` que escuta a URL ``localhost:8080/blog``. Sempre que alguem acessar essa
rota, o método ``indexAction()`` da classe ``Blog\Controller\List`` será executado. Entretanto, esse controller ainda
não existe, então se você recarregar a página irá ver a seguinte mensagem de erro:

.. code-block:: html
   :linenos:

    A 404 error occurred
    Page not found.
    The requested controller could not be mapped to an existing controller class.

    Controller:
    Blog\Controller\List(resolves to invalid controller class or alias: Blog\Controller\List)
    No Exception available

Nos agora precisamos dizer ao nosso modulo onde encontrar  o controller com nome ``Blog\Controller\List``. Para
conseguir isso nos temos que adicionar uma chave a configuração ``controllers`` dentro do arquivo
``/module/Blog/config/module.config.php``.

.. code-block:: php
   :linenos:
   :emphasize-lines: 4-8

    <?php
    // Filename: /module/Blog/config/module.config.php
    return array(
        'controllers' => array(
            'invokables' => array(
                'Blog\Controller\List' => 'Blog\Controller\ListController'
            )
        ),
        'router' => array( /** Route Configuration */ )
    );

Essa configuração define que ``Blog\Controller\List`` é um apelido para ``ListController`` dentro do namespace
``Blog\Controller``. Recarregando a página deve então mostrar a você:

.. code-block:: html
   :linenos:

    ( ! ) Fatal error: Class 'Blog\Controller\ListController' not found in {libPath}/Zend/ServiceManager/AbstractPluginManager.php on line {lineNumber}

Esse erro nos diz que a aplicação sabe qual classe carregar, mas não sabe onde encontra-la. Para corrigir isso, nos
vamos precisar configurar o `autoloading <http://www.php.net/manual/en/language.oop5.autoload.php>`_ do nosso modulo.
Autoloading é o processo de deixar o PHP automaticamente carregar as classes por demanda. Para nosso modulo nos
configuramos isso ao adicionar um método ``getAutoloaderConfig()`` na nossa classe Module. (Essa função é definida
na `AutoloaderProviderInterface <https://github.com/zendframework/zf2/:current_branch/library/Zend/ModuleManager/Feature/AutoloaderProviderInterface.php>`_,
entretanto somente a presença do método é suficiente, implementar a interface de fato é opcional).

.. code-block:: php
   :linenos:
   :emphasize-lines: 5,9

    <?php
    // Filename: /module/Blog/Module.php
    namespace Blog;

    use Zend\ModuleManager\Feature\AutoloaderProviderInterface;
    use Zend\ModuleManager\Feature\ConfigProviderInterface;

    class Module implements
        AutoloaderProviderInterface,
        ConfigProviderInterface
    {
        /**
         * Return an array for passing to Zend\Loader\AutoloaderFactory.
         *
         * @return array
         */
        public function getAutoloaderConfig()
        {
            return array(
                'Zend\Loader\StandardAutoloader' => array(
                    'namespaces' => array(
                        // Autoload all classes from namespace 'Blog' from '/module/Blog/src/Blog'
                        __NAMESPACE__ => __DIR__ . '/src/' . __NAMESPACE__,
                    )
                )
            );
        }

        /**
         * Returns configuration to merge with application configuration
         *
         * @return array|\Traversable
         */
        public function getConfig()
        {
            return include __DIR__ . '/config/module.config.php';
        }
    }

Agora isso parece muitas mudanças mas não se assuste. Nos adicionamos o método ``getAutoloaderConfig()`` que contem
configurações para o  ``Zend\Loader\StandardAutoloader``. Essas configuraes informam a aplicação que classes
no ``__NAMESPACE__`` (``Blog``) podem ser encontradas dentrdo de ``__DIR__ . '/src/' . __NAMESPACE__``
(``/module/Blog/src/Blog``).

O ``Zend\Loader\StandardAutoloader`` usa um padrão desenvolivido pela comunidade chamado de `PSR-0` <https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-0.md>`_.
Entre outras coisas, esse padrão define uma forma para o PHP mapear os nomes de classes com o sistema de arquivos.
Então com isso configurado, a aplicação sabe que nossa classe ``Blog\Controller\ListController`` deve existir em
``/module/Blog/src/Blog/Controller/ListController.php``.

Se você atualizar o navegador agora você verá o mesmo erro, já que mesmo que nos tenhamos configurado o autoloader, nos
ainda precisamos criar a classe controller. Então vamos criar esse arquivo agora:

.. code-block:: php
   :linenos:

    <?php
    // Filename: /module/Blog/src/Blog/Controller/ListController.php
    namespace Blog\Controller;

    class ListController
    {
    }

Recarregando a página ira finalmente resultar em uma nova tela. A nova mensagem de erro se parece com isso:

.. code-block:: html
   :linenos:

    A 404 error occurred
    Page not found.
    The requested controller was not dispatchable.

    Controller:
    Blog\Controller\List(resolves to invalid controller class or alias: Blog\Controller\List)

    Additional information:
    Zend\Mvc\Exception\InvalidControllerException

    File:
    {libraryPath}/Zend/Mvc/Controller/ControllerManager.php:{lineNumber}
    Message:
    Controller of type Blog\Controller\ListController is invalid; must implement Zend\Stdlib\DispatchableInterface

Isso acontece por que nosso controller precisa implementar `Zend\Stdlib\DispatchableInterface <https://github.com/zendframework/zf2/:current_branch/library/Zend/Stdlib/DispatchableInterface.php>`_
para que possa ser 'dispatched' (ou executada) pela camada MVC do by ZendFramework. O ZendFramework fornece algumas
implementações básicas de controllers como `AbstractActionController <https://github.com/zendframework/zf2/:current_branch/library/Zend/Mvc/Controller/AbstractActionController.php>`_,
que nos iremos usar. Vamos então modificar nosso controller agora:

.. code-block:: php
   :linenos:
   :emphasize-lines: 5,7

    <?php
    // Filename: /module/Blog/src/Blog/Controller/ListController.php
    namespace Blog\Controller;

    use Zend\Mvc\Controller\AbstractActionController;

    class ListController extends AbstractActionController
    {
    }

Chegou a hora de um novo recarregamento do site. Você deve ver uma nova mensagem de erro:

.. code-block:: html
   :linenos:

    An error occurred
    An error occurred during execution; please try again later.

    Additional information:
    Zend\View\Exception\RuntimeException

    File:
    {libraryPath}/library/Zend/View/Renderer/PhpRenderer.php:{lineNumber}
    Message:
    Zend\View\Renderer\PhpRenderer::render: Unable to render template "blog/list/index"; resolver could not resolve to a file

Agora a aplicação informa que um arquivo de view não pode ser renderizado, o que é esperado já que nos não criamos um
ainda. A aplicação espera que ele esteja em``/module/Blog/view/blog/list/index.phtml``. Crie esse artigo e adicione
algum conteúdo aleatório a ele:

.. code-block:: html
   :linenos:

    <!-- Filename: /module/Blog/view/blog/list/index.phtml -->
    <h1>Blog\ListController::indexAction()</h1>

Antes de continuarmos vamos rapidamente olhar onde nos colocamos esse arquivo. Note que os arquivos de view são
encontrados dentro do subdiretório ``/view`` não ``/src`` já que eles não são arquivos de classes em PHP, mas sim
arquivos de templates para renderizar HTML. o caminho seguinte entretanto merece algumas explicações mas é muito simples.
Primeiro nos temos o namespace em minusculas. Seguido pelo nome do controller também em minusculas e sem o sufixo
'controller' e por último temos o nome da action que estamos acessando, novamente sem o sufixo 'action'. Tudo isso
se parrce com isso: ``/view/{namespace}/{controller}/{action}.phtml``. Isso se tornou um padrão da comunidade, mas
pode ser alterado por você a qualquer momento.

Entretanto criando somente esse arquivo não é o suficiente e isso nos leva ao último tópico dessa parte do Guia. Nos
precisamos deixar a aplicação saber onde procurar pelos arquivos de view. Nos fazemos isso dentro do arquivo de
configuração do nosso modulo ``module.config.php``.

.. code-block:: php
   :linenos:
   :emphasize-lines: 4-8

    <?php
    // Filename: /module/Blog/config/module.config.php
    return array(
        'view_manager' => array(
            'template_path_stack' => array(
                __DIR__ . '/../view',
            ),
        ),
        'controllers' => array( /** Controller Configuration */),
        'router'      => array( /** Route Configuration */ )
    );

A configuração acima informa a aplicação que a pasta ``/module/Blog/view`` possui arquivos de view que se adequam ao
esquema padrão apresentado acima. É importante notas que com isso você pode não somente criar arquivos de views para
seus modulos como também sobreescrever os arquivos de views de outros módulos.

Recarregue o site agora. Finalmente nos estamos em um ponto onde você pode ver algi diferente de uma mensagem de erro
sendo exibida. Parabens, você não somente acabou de criar um simples modulo no estilo "Hello World" como também
aprendeu sobre muitas mensagens de erros e suas causas. Se você não está exausto, continue com o Guia e vamos criar
um módulo que de fato faça alguma coisa.
