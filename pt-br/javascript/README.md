# JavaScript

O WordPress possui um gerenciador de dependência, que lhe permite controlar os *imports* do JavaScript. Não use a tag `<script>` para fazer *embeds* de scripts, ao invés disso, crie um arquivo separado e o importe.

## Registrando e Importando

Os *scripts* devem ser registrados, isso facilita o trabalho do gerenciador de dependência, pois quando você registra o arquivo, você declara a existência do *script* para o *WordPress*.

Vamos registrar e importar um script.

```php
// Usa a função wp_enqueue_scripts para registrar e importar os scripts no Front-End.
add_action( 'wp_enqueue_scripts', 'register_and_enqueue_a_script' );
function register_and_enqueue_a_script() {
   // Associa uma ID chamada `my-script` para este script em específico.
   //  + este arquivo está localizado na raiz do tema,
   //  + que possui como dependência o o jQuery,
   //  + Um *timestamp* é adicionado toda vez que o arquivo é modificado, isso previne o cache do arquivo durante o desenvolvimento,
   //  + ele é posicionado no final da página (footer)
   wp_register_script(
      'my-script',
      get_template_directory_uri().'/js/functions.js',
      array( 'jquery' ),
      filemtime( get_template_directory().'/js/functions.js',
      true
   );
   // Importamos o script.
   wp_enqueue_script( 'my-script' );
}
```

### Pro-Tips:
- Os scripts devem ser importados apenas quando necessário; Envolvendo a função `wp_enqueue_script()` usando condicionais para controle do mesmo.
- Quando você for importar seus *scripts* no Painel do Admin, use o *hook* `admin_enqueue_scripts`.
- Se for adicionar scripts para a tela de login, use o *hook* `login_enqueue_scripts`.

## Localização

A *localização* é um script que lhe permite passar variáveis(dados) ​​do PHP para JavaScript. Isso normalmente é usado para internacionalização de *strings* (tradução), mas existem muitas outras meneiras de usarmos esta técnica.

De um ponto de vista técnico, localizar um script significa que haverá uma nova tag `<script>` adicionada logo antes dos scripts registrados, que contém o objeto `_global_` JavaScript com o nome que você especificou durante a localização no segundo argumento. Isso significa que, se você adicionar um outro *script* que possui esse mesmo *script* como dependência, você poderá usar o mesmo objeto `_global_` no novo *script* sem problemas. O WordPress trabalha muito bem com encadeamento de dependências.

Vamos *localizar* um script.

```php
add_action( 'wp_enqueue_scripts', 'register_localize_and_enqueue_a_script' );
function register_localize_and_enqueue_a_script() {
   wp_register_script(
      'my-script',
      get_template_directory_uri().'/js/functions.js',
      array( 'jquery' ),
      filemtime( get_template_directory().'/js/functions.js' ),
      true
   );
   wp_localize_script(
      'my-script',
      'scriptData',
      // Estes são os dados, que irão ser mandados para o arquivo JavaScript.
      array(
         'alertText' => 'Are you sure you want to do this?',
      )
   );
   wp_enqueue_script( 'my-script' );
}
```

No arquivo de javascript, os dados estão disponíveis através do objeto especificado durante o processo de *localização*.

```javascript
( function( $, plugin ) {
   alert( plugin.alertText );
} )( jQuery, scriptData || {} );
```

## Remover os importes e registros padrão do WordPress

Você pode remover um registro ou um importe através das funções `wp_deregister_script()` e `wp_dequeue_script()`.

## AJAX

WordPress oferece *endpoints* no servidor para requisições **AJAX**, localizado em `wp-admin/admin-ajax.php`. Vamos configurar um *endpoint* no servidor para manipulação AJAX.

```php
// É acionado quando o usuário está logado no Painel.
add_action( array( 'wp_ajax_nopriv_create_new_post', 'wp_ajax_create_new_post' ), 'wp_ajax_create_new_post_handler' );
function wp_ajax_create_new_post_handler() {
   // Este é um dado que não foi filtrado, validado e tratado.
   $data = $_POST['data'];

   // Faz coisas aqui.
   // Por exemplo: Insere ou atualiza um post.
   $post_id = wp_insert_post( array(
      'post_title' => $data['title'],
   ) );

   // Se tudo der certo, insira qualquer tipo de dado para o callback do JavaScript.
   // Neste exemplo, wp_insert_post() retorna a ID de último post criado.
   // Isso adiciona um `exit`/`die` por si só, então não há necessidade de ser chamada.
   if ( ! is_wp_error( $post_id ) ) {
      wp_send_json_success( array(
         'post_id' => $post_id,
      ) );
   }

   // Se alguma coisa der errado, a última parte será passada e só então executada:
   wp_send_json_error( array(
      'post_id' => $post_id,
   ) );
}


add_action( 'wp_enqueue_scripts', 'register_localize_and_enqueue_a_script' );
function register_localize_and_enqueue_a_script() {
    wp_register_script(
      'my-script',
      get_template_directory_uri().'/js/functions.js',
      array( 'jquery' ),
      filemtime( get_template_directory().'/js/functions.js' ),
      true
    );
    
    // Manda os dados do PHP através de variáveis para o JavaScript.
    wp_localize_script(
      'my-script',
      'scriptData',
      array(
         'ajax_url' => admin_url( 'admin-ajax.php' ),
      )
    );
    
    wp_enqueue_script( 'my-script' );
}
```

Em seguida vem o JavaScript:

```javascript
( function( $, plugin ) {
   $( document ).ready( function() {
      $.post(
         
         plugin.ajax_url,
         {
            // Os nomes especificados são os `triggers` correspondentes aos hookers
            // wp_ajax_* e wp_ajax_nopriv_* do lado do servidor.
            action : 'create_new_post',
            
            // Encapsula todos os dados do lado do servidor em um objeto.
            data : {
               title : 'Hello World'
            }
         },
         
         function( response ) {
            // wp_send_json_success() define a propriedade "sucsess" para verdadeiro
            if ( response.success ) {
               // Qualquer dado que passou pela função wp_send_json_sucesess(), está disponível na propriedade "data".
               alert( 'A post was created with an ID of ' + response.data.post_id );

            // wp_send_json_error() define a propriedade "sucsess" para falso.
            } else {
               alert( 'There was a problem creating a new post.' );
            }
         }

      );
   } );
} )( jQuery, scriptData || {} );
```

`ajax_url` representa o *endpoint* do admin via AJAX, que é automaticamente definida na página do Admin, mas não no front-end.

Vamos localizar nosso script para incluir a URL do admin:

```php
add_action( 'wp_enqueue_scripts', 'register_localize_and_enqueue_a_script' );
function register_localize_and_enqueue_a_script() {
   wp_register_script( 'my-script', get_template_directory_uri() . '/js/functions.js', array( 'jquery' ) );
   // Manda os dados do PHP através de variáveis para o JavaScript.
   $data_for_script = array( 'ajax_url' => admin_url( 'admin-ajax.php' ) );
   wp_localize_script( 'my-script', 'scriptData', $data_for_script );
   wp_enqueue_script( 'my-script' );
}
```

## JavaScript com WP AJAX

Existe várias maneiras de fazermos isto. A maneira mais comum é usar `$.ajax()`. É claro que existem atalhos disponíveis como `$.post()` e `$.getJSON()`.

Aqui esta um exemplo padrão.

```javascript
/*globals jQuery, $, scriptData */
( function( $, plugin ) {
   "use strict";

   // Variação do jQuery.ajax()
   // Você também pode usar o $.post(), $.getJSON() se quiser
   // Eu prefiro declarar explicitamente o carregamento & as promessas descritas abaixo
   $.ajax( {
       url  : plugin.ajaxurl,
       data : {
         action      : plugin.action,
         _ajax_nonce : plugin._ajax_nonce,
         // Objeto-Global do WordPress
         // É mostrado apenas no "admin"
         postType     : typenow,
       },
       beforeSend : function( d ) {
         console.log( 'Before send', d );
       }
   } )
      .done( function( response, textStatus, jqXHR ) {
         console.log( 'AJAX done', textStatus, jqXHR, jqXHR.getAllResponseHeaders() );
      } )
      .fail( function( jqXHR, textStatus, errorThrown ) {
         console.log( 'AJAX failed', jqXHR.getAllResponseHeaders(), textStatus, errorThrown );
      } )
      .then( function( jqXHR, textStatus, errorThrown ) {
         console.log( 'AJAX after finished', jqXHR, textStatus, errorThrown );
      } );
} )( jQuery, scriptData || {} );
```

Perceba que o exemplo acima usa `_ajax_nonce` para verificar o valor NONCE, o qual você terá de definir sozinho, quando for *localizar* o script. É só adicionar `'_ajax_nonce' => wp_create_nonce( "some_value" ),` no objeto *array*. Você então poderá adicionar um marcador nos seus *callbacks* do PHP, que se parecem um pouco com isso `check_ajax_referer( "some_value" )`.

## Trabalhando com AJAX e Eventos

Na verdade é muito simples executar uma requisição AJAX por cliques (ou qualquer outro tipo de interação do usuário) em algum elemento. Apenas envolva a sua chamada `$.ajax()` (ou algum de seu similares). Você pode ainda, adicionar um delay na requisição.

```javascript
$( '#' + plugin.element_name ).on( 'keyup', function( event ) {
   $.ajax( { ... etc ... } )
      .done( function( ... ) { etc }
      .fail( function( ... ) { etc }

} )
   .delay( 500 );
```

## Multiplos callbacks para uma única requisição AJAX

Você pode acabar caindo em uma situação da qual multiplas tarefas acontecem depois de que uma requisição AJAX é finalizada. Felizmente jQuery retorna um objeto, onde você pode anexar todos os seus callbacks.

```javascript
/*globals jQuery, $, scriptData */
( function( $, plugin ) {
   "use strict";

   // Variação do jQuery.ajax()
   // Você também pode usar o $.post(), $.getJSON() se quiser
   // Eu prefiro declarar explicitamente o carregamento & as promessas 
   var request = $.ajax( {
       url  : plugin.ajaxurl,
       data : {
         action      : plugin.action,
         _ajax_nonce : plugin._ajax_nonce,
         // Objeto-Global do WordPress
         // É mostrado apenas no "admin"
         postType     : typenow,
       },
       beforeSend : function( d ) {
         console.log( 'Before send', d );
       }
   } );

   request.done( function( response, textStatus, jqXHR ) {
      console.log( 'AJAX callback #1 executed' );
   } );

   request.done( function( response, textStatus, jqXHR ) {
      console.log( 'AJAX callback #2 executed' );
   } );

   request.done( function( response, textStatus, jqXHR ) {
      console.log( 'AJAX callback #3 executed' );
   } )
} )( jQuery, scriptData || {} );
```

## Encadeamento de Callbacks

Um cenário comum (do qual muitas vezes um *callback* é necessário e como ele é simples de ser utilizado), é o encadeamento de callbacks quando uma requisição AJAX é finalizada.

Vamos dar uma olhada no problema primeiro:

> O callback (A) é executado.
> O callback (B) não sabe que deve esperar por (A).
> Você não pode ver o problema na sua instalação local se (A) terminar rapidamente.

A grande questão é, quando nós devemos esperar (A) finalizar, para só então, inicializarmos o processo de (B).

A resposta é que o processo é "adiado" carregando suas ["promessas"]( http://en.wikipedia.org/wiki/Futures_and_promises ), também conhecido como "futuros".

Veja um exemplo:

```javascript
( function( $, plugin ) {
    "use strict";

    $.when(
        $.ajax( {
            url :  pluginURl,
            data : { /* ... */ }
        } )
           .done( function( data ) {
                // 2nd call finished
           } )
           .fail( function( reason ) {
               console.info( reason );
           } );
    )
    // Novamente, você poderia utliziar o método .done() se quisesse. Veja a documentação do jQuery.
    .then(
        // Sucesso
        function( response ) {
            // Has been successful
            // In case of more then one request, both have to be successful
            // Sucesso! Em caso de mais de uma requisição, ambos deverão ser bem sucedidos.
        },
        // Falhou
        function( resons ) {
            // É jogado um erro, em caso de multiplos erros, é o primeiro que é cuspido.
        },
    );
    //.then( /* and so on */ );
} )( jQuery, scriptData || {} );
```
[_Fonte: WordPress.StackExchange / Kaiser_](http://wordpress.stackexchange.com/a/118796/385)