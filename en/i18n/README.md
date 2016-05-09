# I18n

When talking about I18n here, we're going to talk about translation strings in user interfaces and on the frontend. For content in multiple languages or language pickers for users, you will need to install a plugin to provide the editing tools for posts and other content types.

At any point, you can manually set the language WordPress uses by user or overriding the `WPLANG` option. Older tutorials will recommend the `WP_LANG` constant, but this has been deprecated

However you will need to make sure the necessary language files are in place in your `wp-content/languages` folder before the change takes full effect.

Translation work falls under the Polyglots group at contributor days. If you're interested in translating WordPress Core, [you should read the official translators handbook](https://make.wordpress.org/polyglots/handbook/) to find out how

## Setting the Admin language

On installation WordPress asks you to select a language, but you may want to set different languages for the front end back end. For example a German website ran by an English speaker may want the admin area to be in their native language.

In order to do this, set your sites language to German using the `WP_LANG` constant mentioned earlier, and add this code to set the admin area language to english:

```
add_filter('locale', 'wpse27056_setLocale');
function wpse27056_setLocale($locale) {
    if ( is_admin() ) {
        return 'en_US';
    }

    return $locale;
}
```

## Foreign Twitter Embeds

Sometimes oembeds come back in an unexpected foreign language, this is because the service being used is looking at your servers request and tracing it back to its origin to determine it's country. For example, an English website hosted on a German server may result in German twitter embeds.

## Further Reading

 - [Different languages for front and back ends ](https://wordpress.stackexchange.com/questions/27056/different-language-for-frontend-and-backend)
