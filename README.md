# Bem vindo a documentação do Zend Framework 2 em Português

Essa documentação é baseada na documentação original em inglês disponível em:
https://github.com/zendframework/zf2-documentation


## Veja a Documentação Online

Nós usamos o [readthedocs.org](http://readthedocs.org/) para renderizar online
a versão de desenvolvimento da tradução da documentação do Zend Framework 2.

Você pode ler a documentação em  [http://zf2-documentation-br.readthedocs.org](http://zf2-documentation-br.readthedocs.org/pt/latest/).

## Informações de Compilação

Para compilar a documentação é necessário ter o [Sphinx](http://sphinx-doc.org/). 
Também são necessárias as bibliotecas _Pygments_, _docutils_ and _markupsafe_, 
que podem ser instaladas com `pip install`.

Acesse o diretório `docs/`, e execute `make` com um dos seguintes parâmetros:

- `epub` - build epub (ebook) documentation (required)
  [Calibre](http://calibre-ebook.com/) to build cross-platform epub versions)
- `help` - build Windows help files
- `html` - build HTML documentation
- `info` - build Unix info pages
- `latexpdf` - build PDF documentation (requires a working `latex` toolchain)
- `man` - build Unix manpages
- `text` - build ANSI text manual files

Exemplo:

```sh
make html
```

Nota: No momento somente a versão em HTML é suportada, porém existem planos para
suportar outros formatos

Você pode realizar a limpeza com `make clean`.

## Contribuindo

Caso queira contribuir com a tradução da documentação do Zend Framework 2 por favor
leia o arquivo CONTRIBUTING.md.

Se você não sabe por onde começar, ou onde sua ajuda será melhor aproveitada, por
favor leia o arquivo TODO.md.

## Licença

Os arquivos nesse repositório estão licenciados sobre a "Zend Framework License".
Uma cópia dessa licença pode ser encontrada em LICENSE.txt.

## Agradecimentos

O Time do Zend Framework e o time de Tradução agradece a todos os
[contribuidores](https://github.com/zendframework/zf2-documentation/contributors) 
da documentação do Zend Framework e de sua tradução, aos patrocinadores e a você,
usuário do Zend Framework. por favor visite-nos em http://framework.zend.com.
