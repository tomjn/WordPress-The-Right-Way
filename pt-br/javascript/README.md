# JavaScript

O WordPress possui um gerenciador de dependência, que lhe permite controlar os *imports* do JavaScript. Não use a tag `<script>` para fazer *embeds* de scripts, ao invés disso, crie um arquivo separado e o importe.

## Registrando e Importando

Os *scripts* devem ser registrados, isso facilita o trabalho do gerenciador de dependência, pois quando você registra o arquivo, você anuncia a sua existência para o *WordPress*. Para fazer um *embed* de um *script* na página, ele deve ser importado primeiro.

Vamos registrar e importar um script.

```php
// Use the wp_enqueue_scripts function for registering and enqueueing scripts on the front end.
add_action( 'wp_enqueue_scripts', 'register_and_enqueue_a_script' );
function register_and_enqueue_a_script() {
   // Register a script with a handle of `my-script`
   //  + that lives inside the theme folder,
   //  + which has a dependency on jQuery,
   //  + where the UNIX timestamp of the last file change gets used as version number
   //    to prevent hardcore caching in browsers - helps with updates and during dev
   //  + which gets loaded in the footer
   wp_register_script(
      'my-script',
      get_template_directory_uri().'/js/functions.js',
      array( 'jquery' ),
      filemtime( get_template_directory().'/js/functions.js',
      true
   );
   // Enqueue the script.
   wp_enqueue_script( 'my-script' );
}
```

### Pro-Tips:
- Os scripts devem ser importados apenas quando necessário; Envolvendo a função `wp_enqueue_script()` usando condicionais para controle.
- Quando você for importar seus *scripts* no Painel do Admin, use o *hook* `admin_enqueue_scripts`.
- Se for adicionar scripts para a tela de login, use o *hook* `login_enqueue_scripts`.

## Localizing

A *localizing* um script permite a você, passar variáveis ​​do PHP para JavaScript. Isso normalmente é usado para internacionalização de *strings*, mas existem muitas outras meneiras de usarmos esta técnica.

De um ponto de vista técnico, localizar um script significa que haverá uma nova tag `<script>` adicionada antes do seu script registrado, que contém o objeto JavaScript `_global_` com o nome que você especificou no segundo argumento. Isso significa também que se você adicionar um outro *script*, que possui esse mesmo *script* como dependência, você poderá usar este mesmo objeto neste novo *script* sem problemas. O WordPress trabalha muito bem com encadeamento de dependências.

Vamos *localize* um script.

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
      // This is the data, which gets sent in localized data to the script.
      array(
         'alertText' => 'Are you sure you want to do this?',
      )
   );
   wp_enqueue_script( 'my-script' );
}
```


No arquivo de javascript, os dados estão disponíveis no nome do objeto especificado enquanto localiza.

```javascript
( function( $, plugin ) {
   alert( plugin.alertText );
} )( jQuery, scriptData || {} );
```

## Cancelamento do registro e ou importe

Você pode cancelar um registro ou um importe de script através das funções `wp_deregister_script()` e `wp_dequeue_script()`.

## AJAX

WordPress tem suporte para chamadas AJAX do lado do servidor, localizado em `wp-admin/admin-ajax.php`. Vamos configurar um *endpoint* no lado do servidor para manipulação AJAX.

```php
// Triggered for users that are logged in.
add_action( 'wp_ajax_create_new_post', 'wp_ajax_create_new_post_handler' );
// Triggered for users that are not logged in.
add_action( 'wp_ajax_nopriv_create_new_post', 'wp_ajax_create_new_post_handler' );

function wp_ajax_create_new_post_handler() {
   // This is unfiltered, not validated and non-sanitized data.
   // Prepare everything and trust no input
   $data = $_POST['data'];

   // Do things here.
   // For example: Insert or update a post
   $post_id = wp_insert_post( array(
      'post_title' => $data['title'],
   ) );

   // If everything worked out, pass in any data required for your JS callback.
   // In this example, wp_insert_post() returned the ID of the newly created post
   // This adds an `exit`/`die` by itself, so no need to call it.
   if ( ! is_wp_error( $post_id ) ) {
      wp_send_json_success( array(
         'post_id' => $post_id,
      ) );
   }

   // If something went wrong, the last part will be bypassed and this part can execute:
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
    // Send in localized data to the script.
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
         // Localized variable, see example below.
         plugin.ajax_url,
         {
            // The action name specified here triggers
            // the corresponding wp_ajax_* and wp_ajax_nopriv_* hooks server-side.
            action : 'create_new_post',
            // Wrap up any data required server-side in an object.
            data   : {
               title : 'Hello World'
            }
         },
         function( response ) {
            // wp_send_json_success() sets the success property to true.
            if ( response.success ) {
               // Any data that passed to wp_send_json_success() is available in the data property
               alert( 'A post was created with an ID of ' + response.data.post_id );

            // wp_send_json_error() sets the success property to false.
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
   // Send in localized data to the script.
   $data_for_script = array( 'ajax_url' => admin_url( 'admin-ajax.php' ) );
   wp_localize_script( 'my-script', 'scriptData', $data_for_script );
   wp_enqueue_script( 'my-script' );
}
```

## Usando JavaScript com WP AJAX

Existe várias maneira de fazermos isto. A maneira mais comum é usar `$.ajax()`. É claro que existem atalhos disponíveis como `$.post()` e `$.getJSON()`.

Aqui esta um exemplo padrão.

```javascript
/*globals jQuery, $, scriptData */
( function( $, plugin ) {
   "use strict";

   // Alternate solution: jQuery.ajax()
   // One can use $.post(), $.getJSON() as well
   // I prefer defered loading & promises as shown above
   $.ajax( {
       url  : plugin.ajaxurl,
       data : {
         action      : plugin.action,
         _ajax_nonce : plugin._ajax_nonce,
         // WordPress JS-global
         // Only set in admin
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

Perceba que o exemplo acima usa `_ajax_nonce` para verificar o valor NONCE, o qual você terá de definir sozinho, quando for localizar um script. Apenas adicione `'_ajax_nonce' => wp_create_nonce( "some_value" ),` no *array* do script. Você então poderá adicionar um marcador nos seus *callbacks* no PHP que se parecem um pouco com isso `check_ajax_referer( "some_value" )`.

## Trabalhando com AJAX e Eventos

Na verdade é muito simples executar uma requisição AJAX por cliques (ou qualquer outro tipo de interação do usuário) em algum elemento. Apenas envolva o seu `$.ajax()` ou algum método similar. Você tem a possibilidade de adicionar um delay na requisição.

```javascript
$( '#' + plugin.element_name ).on( 'keyup', function( event ) {
   $.ajax( { ... etc ... } )
      .done( function( ... ) { etc }
      .fail( function( ... ) { etc }

} )
   .delay( 500 );
```

## Multiplos callbacks para uma única requisição AJAX

Você pode acabar caindo numa situação na qual multiplas coisas acontecem depois de uma requisição AJAX é finalizada. Felizmente jQuery retorna um objeto, onde você pode armazenar todos os seus callbacks.

```javascript
/*globals jQuery, $, scriptData */
( function( $, plugin ) {
   "use strict";

   // Alternate solution: jQuery.ajax()
   // One can use $.post(), $.getJSON() as well
   // I prefer defered loading & promises as shown above
   var request = $.ajax( {
       url  : plugin.ajaxurl,
       data : {
         action      : plugin.action,
         _ajax_nonce : plugin._ajax_nonce,
         // WordPress JS-global
         // Only set in admin
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

Um cenário comum (a respeito de como muitas vezes ela é necessária e como ela é simples), é o encadeamento de callbacks quando uma requisição AJAX é finalizada.

Vamos dar uma olhada no problema primeiro:

> O callback (A) é executado.
> O callback (B) não sabe que deve esperar por (A).
> Você não pode ver o problema na sua instalação local se (A) terminar rapidamente.

A grande questão é, quando nós devemos esperar (A) finalizar para só então, inicializarmos o processo de (B).

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
    // Again, you could leverage .done() as well. See jQuery docs.
    .then(
        // Success
        function( response ) {
            // Has been successful
            // In case of more then one request, both have to be successful
        },
        // Fail
        function( resons ) {
            // Has thrown an error
            // in case of multiple errors, it throws the first one
        },
    );
    //.then( /* and so on */ );
} )( jQuery, scriptData || {} );
```
[_Source: WordPress.StackExchange / Kaiser_](http://wordpress.stackexchange.com/a/118796/385)