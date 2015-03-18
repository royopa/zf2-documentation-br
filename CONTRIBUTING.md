# CONTRIBUTING

Para contribuir com a tradução da documentação do Zend Framework 2 para Português do Brasil
você pode enviar um [pull request](https://help.github.com/articles/using-pull-requests)
através do GitHub, Só isso!

Se você não estiver familiarizado com o GitHub você pode ler o
[Zend Framework Git Guide](https://github.com/zendframework/zf2/blob/master/README-GIT.md)
usando o repositório `git@github.com:PHPSP/zf2-documentation-br.git` ao inves de
`git://github.com/zendframework/zf2.git` (Em inglês).

Nos somente pedimos que siga algumas convenções:

 - Se você quiser propor uma nova documentação, faça isso primeiramente no [repositório
   original em inglês](https://github.com/zendframework/zf2-documentation);
      
 - Se o arquivo já existir no repositório em ingles mas ainda não no em português use
   o nome da sua branch seguindo a convenção de `feature/<doc>`, onde o `<doc>` 
   é o nome do documento que você deseja criar;

 - Para corrigir ou atualizar um socumento nomeie sua branch como `fix/<doc>`,
   onde `<doc>` é o nome do documento que você deseja corrigir ou atualizar;

Para traduções para outros idiomas por favor dirija-se ao repositório oficial em
inglês para obter informações sobre os projetos de tradução específicos.

## Formato dos Documentos

A documentação do Zend Framework 2, assim como essa tradução, são escritas usando
o formato [reStructuredText](http://en.wikipedia.org/wiki/ReStructuredText).

Se você não estiver familiarizado com o formato nós sugerimos que você veja
[reStructuredText primer](http://sphinx.pocoo.org/rest.html).

Para renderizar a documentação nos usamos o Projeto [Sphinx](http://sphinx.pocoo.org/.
No momento somente o formato **HTML** é suportado mas existem planos para dar suporte
a outros formatos.

Existe um limite flexível de 120 caracteres por linha. Dessa forma é nos recomendamos
que você se limite a isso sempre que possível.

## Renderizando a Documentação

Para renderizar a documentação você deve ter instalado o [Sphinx](http://sphinx.pocoo.org/) 
versão 1.1.0 ou superior. Sphinx precisa de Python e algumas bibliotecas Python para funcionar.

Se o seu sistema já contem um pacote com Sphynx, certifique-se que seja uma versão igual
ou superior a 1.1. No momento da escrita desse arquivo algumas versões do Debian fornecem o 
Sphynx nas versões 0.6 ou 1.0.8 e não irá funcionar corretamente com essa documentação.

Para instalar a ultima versão estável do Sphynx em distribuições baseadas em Debian
faça os procedimentos a seguir:

    > apt-get install python-setuptools python-pygments
    > easy_install -U Sphinx
    > sphinx-build --version
      Sphinx v1.1.3
      Usage: /usr/local/bin/sphinx-build ....

Para renderizar a documentação em formato HTML faça da seguinte forma:

 1. Clone o [repositório git](git@github.com:PHPSP/zf2-documentation-br.git) 
 ou faça o download e extraia o arquivo da documentação.
 1. Acesse o subdiretório `docs/`;
 1. Execute `make html`

A documentação em HTML será gerada em `docs/_build/html`.

Por padrão o comando `make html` já irá renderizar a documentação em Português.

Após realizar as alterações nos arquivos .rst você poderá executar `make html`
novamente para atualizar o HTML com as alterações realizadas.