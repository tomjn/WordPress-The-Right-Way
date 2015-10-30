# Guia de estilo de código

É importante manter o código legível e de fácil manutenção. Isso previne que pequenos erros, tornem-se críticos a ponto de você perder horas de trabalho, para perceber que esqueceu de adicionar um ";" na Linha X. É recomendável usar o mesmo "Code Standard" ao decorrer de toda a sua aplicação. Mas se você se sente mais confortável, usando o padrão PSR, não há problema algum nisso, com tanto que a use de maneira consistente.

### Indentação

Indentação no WordPress é feito usando "tabs", representado por 4 espaços. Indentação é muito importante para um código mais legível, e cada declaração deve estar em sua própria linha. Sem a indentação, fica mais díficil de entender o código, e os erros tornam-se mais frequentes.

Dê uma olhada em [Editor Config](http://editorconfig.org). Ele é uma ótima maneira de assegurar de que todos os membros do time, estão seguindo UM mesmo padrão.

O seguinte arquivo `.editorconfig` reforça as regras acima, com indentação de largura de 4 espaços.

```ini
[*.php]
indent_style = tabs
indent_size = 4
```

### Tag Spam do PHP

As tags `<?php` and `?>` devem ser usadas separadamente. Por exemplo:

```php
<?php // bad ?>
<?php while( have_posts() ) { ?>
    <?php the_post(); ?>
    <?php the_title(); ?>
    <strong><?php the_date(); ?></strong>
    <?php the_content(); ?>
<?php } ?>
```
Seria mais fácil de ler:

```php
<?php
// good
while( have_posts() ) {
    the_post();
    the_title();
    ?>
    <strong><?php the_date(); ?></strong>
    <?php
    the_content();
} ?>
```

### Linting

Muitos editores suportam ou já possuem um verificador de sintaxes. Esses verificadores são chamados de Linters, quando usado com um bom editor, erros de sintaxe são destacados e depurados mais facilmente. Por exemplo, no PHPStorm, é dado a um erro de sintaxe um destaque vermelho.

## Padrões do WordPress

WordPress segue um conjunto de padrões. Esses padrões são diferentes do padrão PSR. Por exemplo, WordPress usa tabs ao invés de espaços, e abre os parentêses em uma nova linha.

O Manual do WordPress para contribuidores, aborda os padrões de código, mais detalhadamente. Abaixo estão algumas referências para estudo.

- [HTML Coding Standards](http://make.wordpress.org/core/handbook/coding-standards/html/)

- [PHP Coding Standards](http://make.wordpress.org/core/handbook/coding-standards/php/)

- [JavaScript Coding Standards](http://make.wordpress.org/core/handbook/coding-standards/javascript/)

- [CSS Coding Standards](http://make.wordpress.org/core/handbook/coding-standards/css/)


#### PHP Code Sniffer & PHP CS Fixer

O PHP Code Sniffer é uma ferramente responsável por verificar violações no código. Atualmente muitos editores, trazem com eles essa função. E o PHP CS Fixer, corrigi automaticamente essas violações para você.

Para usar essas ferramentas, você precisará da definição dos `Padrões de Código` do WordPress. [Você pode encontra-los aqui, junto com as instruções do PHPStorm](https://gist.github.com/Rarst/1370155)
