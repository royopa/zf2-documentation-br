.. _introduction.installation:

**********
Instalação
**********

.. _installation.composer:

Usando Composer
---------------

A forma recomendada de iniciar um novo projeto com Zend Framework é clonar a aplição
esqueleto e usar o ``composer`` para instalar as dependencias usando o comando ``create-project``:

.. code-block:: text
   :linenos:

   curl -s https://getcomposer.org/installer | php --
   php composer.phar create-project -sdev --repository-url="https://packages.zendframework.com" zendframework/skeleton-application path/to/install

Como alternativa, clonar o repositório e mnualmente chamar o ``composer`` usando o arquivo composer.phar
incluído no repositório:

.. code-block:: text
   :linenos:

   cd my/project/dir
   git clone git://github.com/zendframework/ZendSkeletonApplication.git
   cd ZendSkeletonApplication
   php composer.phar self-update
   php composer.phar install

(A instrução ``self-update`` é usada para garantir que você tenha um arquivo ``composer.phar``
atualizado.)

Outra alternativa para fazer o download do projeto e baixa-lo com `curl`, e descompacta-lo com `tar`:

.. code-block:: text
   :linenos:

   cd my/project/dir
   curl -#L https://github.com/zendframework/ZendSkeletonApplication/tarball/master | tar xz --strip-components=1

Você então deverá chamar o ``composer`` para instalar as dependencias assim como no exemplo anterior.

.. _installation.git.submodules:

Usando Git submodules
---------------------
Alternativamente, você pode instalar usando a ferramenta nativa git submodules:

.. code-block:: text
   :linenos:

   git clone git://github.com/zendframework/ZendSkeletonApplication.git --recursive

Confĩgurações de Servidor Web
-----------------------------

Servidor PHP CLI
^^^^^^^^^^^^^^^^

O jeito mais simples de começar se você estiver usando PHP 5.4 ou superior é iniciar
o servidor interno do PHP no diretório raiz:

.. code-block:: text
   :linenos:

   php -S 0.0.0.0:8080 -t public/ public/index.php

Dessa forma você irá iniciar o servidor cli na porta 8080 e fixar ele em todas as interfaces de rede.

.. note::

   O servidor embiutido do PHP deve ser usado *apenas para desenvolvimento*.

Configurando o Apache
^^^^^^^^^^^^^^^^^^^^^

Para usar o apache, configure um servidor virtual apontando para o diretório ``public/`` do projeto.
Ele deve se parecer com o seguinte:

.. code-block:: text
   :linenos:

   <VirtualHost *:80>
      ServerName zf2-tutorial.localhost
      DocumentRoot /path/to/zf2-tutorial/public

      <Directory /path/to/zf2-tutorial/public>
          AllowOverride All
          Order allow,deny
          Allow from all
      </Directory>
   </VirtualHost>

ou se você estiver usando Apache 2.4 ou superior:

.. code-block:: text
   :linenos:

   <VirtualHost *:80>
      ServerName zf2-tutorial.localhost
      DocumentRoot /path/to/zf2-tutorial/public

      <Directory /path/to/zf2-tutorial/public>
          AllowOverride All
          Require all granted
      </Directory>
   </VirtualHost>

.. _installation.rewrite.configuration:

Configuração de Reescrita
,,,,,,,,,,,,,,,,,,,,,,,,,

Reescrita de *URL* é uma função comum nos servidores *HTTP*, e possibilita que todas as requisições HTTP sejam roteadas
atravez do ponto de entrada ``index.php`` de uma aplicação Zend Framework.

O apache vem integrado com um modulo chamado ``mod_rewrite`` para reescrita de URL. Para usa-lo, ``mod_rewrite`` precisa
ou ser incluído em tempo de compilação ou ser habilitado como um Dynamic Shared Object (*DSO*). Por favor consulte a
`Documentação do Apache`_ da sua versão para mais informações.

A aplicação esqueleto do Zend Framework inclui um arquivo ``.htaccess`` com as regras de reescrita que irão cobrir a
maioria dos casos de uso:

.. code-block:: text
   :linenos:

   RewriteEngine On
   # The following rule tells Apache that if the requested filename
   # exists, simply serve it.
   RewriteCond %{REQUEST_FILENAME} -s [OR]
   RewriteCond %{REQUEST_FILENAME} -l [OR]
   RewriteCond %{REQUEST_FILENAME} -d
   RewriteRule ^.*$ - [NC,L]
   # The following rewrites all other queries to index.php. The
   # condition ensures that if you are using Apache aliases to do
   # mass virtual hosting, the base path will be prepended to
   # allow proper resolution of the index.php file; it will work
   # in non-aliased environments as well, providing a safe, one-size
   # fits all solution.
   RewriteCond %{REQUEST_URI}::$1 ^(/.+)(.+)::\2$
   RewriteRule ^(.*) - [E=BASE:%1]
   RewriteRule ^(.*)$ %{ENV:BASE}index.php [NC,L]

.. _installation.iis:

Microsoft Internet Information Services
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

A partir da versão 7.0, *IIS* vem com um motor de reescrita padrão. Você pode usar a seguinte configuração para
criar as regras apropriadas de reescrita.

.. code-block:: xml
   :linenos:

   <?xml version="1.0" encoding="UTF-8"?>
   <configuration>
       <system.webServer>
           <rewrite>
               <rules>
                   <rule name="Imported Rule 1" stopProcessing="true">
                       <match url="^.*$" />
                       <conditions logicalGrouping="MatchAny">
                           <add input="{REQUEST_FILENAME}"
                                matchType="IsFile" pattern=""
                                ignoreCase="false" />
                           <add input="{REQUEST_FILENAME}"
                                matchType="IsDirectory"
                                pattern=""
                                ignoreCase="false" />
                       </conditions>
                       <action type="None" />
                   </rule>
                   <rule name="Imported Rule 2" stopProcessing="true">
                       <match url="^.*$" />
                       <action type="Rewrite" url="index.php" />
                   </rule>
               </rules>
           </rewrite>
       </system.webServer>
   </configuration>

.. _`Documentação do Apache`: http://httpd.apache.org/docs/
