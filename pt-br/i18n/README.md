# I18n

Quando nós falamos sobre I18n, nós estamos falando sobre traduções de `strings` em interfaces de usuários e ou no frontend. Para apresentarmos conteúdo em multiplos idiomas ou para gerar um sistema, que seleciona o idioma com base no país do usuário, você precisará instalar um `plugin` que possui ferramentas de edição linguística para `posts` e ou qualquer outro tipo de conteúdo como: Páginas, Categorias, etc.

A qualquer momento, você pode alterar manualmente o idioma do WordPress, mudando a constante `WP_LANG` no `wp-config.php`, e.g:

```
define ('WPLANG', 'zh_CN');
```

No entanto, você precisa ter certeza de que o arquivo do idioma foi colocado corretamente no diretório `wp-content/languages` antes de fazer tal alteração.

As traduções nunca funcionam, dentre os grupos de poliglotas durante os "Dias de Contribuidor". Se você esta interessado em traduzir o `Core` do WordPress, [você deveria dar uma olhada no HandBook oficial para tradutores](https://make.wordpress.org/polyglots/handbook/) e descobrir como ajudar.

## Configurando o Idioma do Admin

No processo de instalação do WordPress, um dos passos é a seleção do idioma, porém você talvez queira definir idiomas diferentes para o frontend e outro para o backend. Por exemplo um site alemão que é administrado por um falante da lingua inglesa, talvez queira a area do admin em sua lingua nativa.

Para fazer isso, defina o idioma para Alemão na constante `WP-LANG` mencionada anteriormente, e adicione este código para deixar a area do admin em ingles:

```
add_filter('locale', 'wpse27056_setLocale');
function wpse27056_setLocale($locale) {
    if ( is_admin() ) {
        return 'en_US';
    }

    return $locale;
}
```

## Embeds do Twitter para Estrangeiros

As vezes os `embeds` aparecem inesperadamente em uma lingua estrangeira, isso porque tal serviço que esta hospedado em seu servidor, esta realizando uma `request` do servidor de seu país de origem. Por exemplo, um site em ingles hospedado em um servidor Alemão, resultará em um embed do Twitter em alemão.

## Mais detalhes

 - [Idiomas diferentes para frontend e backend ](https://wordpress.stackexchange.com/questions/27056/different-language-for-frontend-and-backend)
