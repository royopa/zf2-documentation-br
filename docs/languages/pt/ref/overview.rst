.. _introduction.overview:

***********
Visão Geral
***********

O Zend Framework 2 é um framework de código aberto para desenvolvimento de aplicações web e serviços usando
*PHP* 5.3+. o Zend Framework 2 usa código 100% `orientado a objetos`_ e a maioria das novas funcionalidades
do PHP 5.3, incluindo `namespaces`_, `late static binding`_, `funções lambda e closures`_.

O Zend Framework 2 é a evolução do Zend Framework 1, um framework PHP de sucesso com mais de 15 milhões de
downloads.

.. note::

    *ZF2* não possui retrocompatibilidade com o *ZF1*, por causa das novas funcionalidades do PHP 5.3+ implementadas
    pelo frameworks, e devido a grandes mudanças em muitos componentes.

A estrutura de componentes do Zend Framework 2 é única ; cada componente e planejado com poucas
dependencias de outros componentes. ZF2 segue os principios de arquitetura orientada a objetos `SOLID`_. Essa
arquitetura com baixo acoplamento permite que desenvolvedores usem quaisquer componentes que quiserem. Nos chamamos
isso de arquitetura "use-at-will". Nos suportamos `Pyrus`_ e `Composer`_ como mecanismos de instalação e gerenciamento
de dependencias para o fraemwork como um todo e para cada componente, melhorando ainda mais essa abordagem.

s Nos usamos `PHPUnit`_ para testar nosso código e `Travis CI`_ como um serviço de Integração Continua.

Apesar de poderem ser usados separadamente, os componentes do Zend Framework 2 na sua apresentação padrão formam um
poderoso e extensível framework de aplicações web quando combinados. Além disso, ele oferece uma implementação robusta
e de alta performance do modelo `MVC`_, uma abstração de banco de dados qu é simples de usar e um componente de
formulario que implementa a `Renderização de formulários em HTML5`_, validação, e filtros para que os desenvolvedores
possam consolidar todas essas operações em uma interface facil de usar e orientada a objetos. outros componentes como
:doc:`Zend\\Authentication <zend.authentication.introduction>` e
:doc:`Zend\\Permissions\\Acl <zend.permissions.acl.introduction>`, fornecem autenticação e autorização de usuários
nos cenários mais comuns de armazenamento de credenciais.

Outros componentes ainda, com o namespace ``ZendService``, implementam bibliotecas clientes para simplificar acesso
aos mais poulares serviços web disponíveis. Sejam quais forem as necessidades de sua aplicação, você provavalmente irá
encontrar um componente do Zend Framework 2 que pode ser usado para resuzir drasticamente o tempo de desenvolvimento com
uma base fortemente testada.
 
O principal patrocinador do projeto 'Zend Framework 2' é a `Zend Technologies`_, ma smuitas outras empresas tem contribuido
com componentes ou funcionalidades significativas para o framework. Empresas como Google, Microsoft e StrikeIron tem sido
parceiras da Zend para fornecer interfaces para os serviços web e outras tecnologias que eles querem que estejam
disponíveis para desenvolvedores Zend Framework 2.

O Zend Framework 2 não poderia entregar e atender todas essas funcionalidades sem a ajuda da vibrante comunidade
do Zend Framework 2. membros da comunidade, incluindo contribuidores, se colocam a disposição nas `listas de e-mails`_,
`canais do IRC`_ e outros forums. Qualquer que seja a dúvida que você tenha sobre o Zend Framework 2, a comunidade
estará sempre disposta a resolver.

.. _`orientado a objetos`: http://en.wikipedia.org/wiki/Object-oriented_programming
.. _`namespaces`: http://php.net/manual/en/language.namespaces.php
.. _`late static binding`: http://php.net/lsb
.. _`funções lambda e closures`: http://php.net/manual/en/functions.anonymous.php
.. _`SOLID`: http://en.wikipedia.org/wiki/SOLID_%28object-oriented_design%29
.. _`Pyrus`: http://pear.php.net/manual/en/pyrus.php
.. _`Composer`: http://getcomposer.org/
.. _`PHPUnit`: http://www.phpunit.de
.. _`Travis CI`: http://travis-ci.org/
.. _`MVC`: http://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller#PHP
.. _`Renderização de formulários em HTML5`: http://www.w3.org/TR/html5/forms.html#forms
.. _`Zend Technologies`: http://www.zend.com
.. _`listas de e-mails`: http://framework.zend.com/archives
.. _`canais do IRC`: http://www.zftalk.com
