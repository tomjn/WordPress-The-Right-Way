# Começando

## PHP Básico

Estamos partindo da ídeia de que você já possui conhecimento básico em PHP. Incluindo alguns tópicos básicos como:

 - [functions](http://www.php.net/manual/en/language.functions.php)
 - [arrays](http://www.php.net/manual/en/language.types.array.php)
 - [variáveis](http://www.php.net/manual/en/language.variables.php)
 - [loops e condições](http://www.php.net/manual/en/language.control-structures.php)
 - [classes e objetos](http://www.php.net/manual/en/language.oop5.php)
 - [herança de classes](http://www.php.net/manual/en/language.oop5.inheritance.php)
 - [polimorfismo](http://code.tutsplus.com/tutorials/understanding-and-applying-polymorphism-in-php--net-14362)
 - [POST](http://www.php.net/manual/en/reserved.variables.post.php) and [GET](http://www.php.net/manual/en/reserved.variables.get.php)
 - [escopo de variáveis](http://www.php.net/manual/en/language.variables.scope.php)

Tenha certeza de que você possui conhecimentos sólidos, nos tópicos abordados acima, antes de continuar. Também estamos assumindo que você possua um editor de texto que destaque a sintaxe do PHP, embora os itens abaixo sejam benéficos.

 - Idêntação Automática
 - Preenchimento Automática
 - Fechamento automático de Parênteses
 - Verificador de Sintaxe

## Ambientes de Desenvolvimento Local

It's important to have a local development environment. Gone are the old days of changing a PHP file then updating it on the live server and hoping for the best.

É importante ter um Ambiente de Desenvolvimento Local. Já se foram os dias, em que tinhamos de fazer UPLOAD em produção, de arquivos PHP, esperando que o melhor aconteça.

Com ambientes locais, você pode trabalhar mais rapidamente, sem ficar fazendo Download e Upload de arquivos, ficando refém de uma internet problemática, ou esperar por todas as páginas carregarem. Com um Servidor Web Local, você pode trabalhar de um trêm em um túnel, sem estar conectado a Wifi ou Plano de Dados, e testar seu trabalho antes de fazer Deploy em produção.

Aqui estão algumas opções para configurar seu Ambiente de Desenvolvimento Local. As duas categorias principais são:

 - VMs (Máquinas Virtuais)
 - LAMPs (Linux, Apache, MySQL e PHP)

O primeiro tipo de ambiente, geralmente envolve terceiros, tais como Vagrant, que oferece uma Máquina Virtual pré-configurada e consistente para se trabalhar.

O segundo tipo instala um programa do servidor, diretamente no Sistema Operacional. Existem várias ferramentas para tornar isto mais fácil, tais como (WAMP, MAMP, XAMPP, VERTRIGO, etc...) mas seu ambiente será único e mais difícil de depurar. Isso as vezes são chamados de LAMP, que traz consigo o Linux, Apache, MySQL e PHP.

### IIS

Microsoft Internet Information Services é um programa de servidor, que roda em servidores Windows. Variantes desse sistema vem com o Windows se você instalar os componentes apropriadamente, mas o conhecimento em desenvolvimento IIS com WP na comunidade WordPress é raro. A maioria dos servidores hoje, rodam com Apache ou Nginx, e o conhecimento dos desenvolvedores tendem a ser direcionados para tais ferramentas. A curva de aprendizado com IIS é mais complexa, e talvez não traga grandes resultados.

## Controladores de Versão

A vital part of working in teams and contributing is version control. Version control systems track changes over time and allow developers to collaborate and undo changes.

Uma parte vital do trabalho em grupo, organização de tarefas e projetos "Open Source" é o uso de um Controlador de Versão. Ele é sistema que monitora as mudanças do seu código em tempo real, e permite, aos desenvolvedores (de sua equipe) a colaborarem e aprimorarem sua aplicação.

### Git

Criado por Linus Torbalds o criador do Linux, [Git é um dos Controladores de Versão mais populares hoje em dia](http://git-scm.com/).

### Subversion

Também conhecido como SVN é uma outra opção, tratando-se de Controladores de Versão, ele é usado nos Plugins e Repositórios do WordPress.org