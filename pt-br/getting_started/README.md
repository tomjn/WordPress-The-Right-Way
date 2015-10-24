# Por onde começar?

## PHP Básico

Estamos partindo da ídeia de que você já possui um conhecimento básico de PHP. Incluindo alguns tópicos básicos como:

 - [Functions](http://www.php.net/manual/en/language.functions.php)
 - [Arrays](http://www.php.net/manual/en/language.types.array.php)
 - [Variáveis](http://www.php.net/manual/en/language.variables.php)
 - [Loops e Condições](http://www.php.net/manual/en/language.control-structures.php)
 - [Classes e Objetos](http://www.php.net/manual/en/language.oop5.php)
 - [Herança de Classes](http://www.php.net/manual/en/language.oop5.inheritance.php)
 - [Polimorfismo](http://code.tutsplus.com/tutorials/understanding-and-applying-polymorphism-in-php--net-14362)
 - [POST](http://www.php.net/manual/en/reserved.variables.post.php) and [GET](http://www.php.net/manual/en/reserved.variables.get.php)
 - [Escopo de variáveis](http://www.php.net/manual/en/language.variables.scope.php)

Tenha certeza de que você possui conhecimentos sólidos nos tópicos abordados acima antes de continuar, também estamos assumindo que você possui um editor de texto que suporte o PHP, recomenda-se que seu editor possua as seguintes "features".

 - Idêntação automática
 - Preenchimento automático
 - Fechamento automático de parênteses
 - Verificador de sintaxe

## Ambientes de Desenvolvimento Local (Localhost)

É importante possuir um `localhost`. A época em que tinhamos de fazer `upload` da aplicação em produção, esperando que o melhor aconteça já é passado, hoje felizmente possuimos algo chamado `Local Web Server` ou Ambiente de Desenvolvimento Local.

Com ambientes locais você pode trabalhar mais rapidamente, sem ficar fazendo `download e upload` de arquivos, ficando refém de uma internet problemática, ou esperar por todas as páginas carregarem. Com um `localhost`, você pode trabalhar de um trêm, ou de um túnel, sem estar conectado a `Wifi ou Plano de Dados`, e testar a aplicação antes de fazer `deploy` em produção.

Aqui estão algumas opções para configurar seu `localhost`. As duas opções mais populares são as/os:

 - VMs (Máquinas Virtuais)
 - LAMPs (Linux, Apache, MySQL e PHP)

O primeiro tipo de ambiente, geralmente envolve terceiros, tais como [Vagrant](https://www.vagrantup.com), que oferece uma "máquina virtual" pré-configurada e consistente para trabalhar.

O segundo tipo instala um programa de servidor diretamente no sistema operacional. Existem várias ferramentas para tornar isto mais fácil, tais como `WAMP, MAMP, XAMPP e ou Vertrigo`. Mas seu ambiente será único e mais difícil de depurar. Estes ambientes são chamados de `LAMP`, que traz consigo `Linux, Apache, MySQL e PHP`.

### IIS

Microsoft Internet Information Services é um programa de servidor que roda em servidores Windows. Variantes desse sistema vem com o Windows se você instalar os componentes apropriadamente, mas o conhecimento em desenvolvimento IIS com WordPress na comunidade é raro. A maioria dos servidores hoje, rodam com Apache ou Nginx, e o conhecimento dos desenvolvedores tendem a ser direcionados para tais ferramentas. A curva de aprendizado com IIS é maior, e talvez não traga grandes resultados.

## Controladores de Versão

Uma parte vital do trabalho em grupo, organização de tarefas e projetos "Open Source", é o uso de um Controlador de Versão. Ele é sistema que monitora as mudanças do seu código em tempo real, e permite, aos desenvolvedores (de sua equipe ou comunidade) a colaborarem e aprimorarem sua aplicação.

### Git
Criado por Linus Torvalds o criador do Linux, [Git](http://git-scm.com/) é um dos controladores de versão mais populares hoje em dia.

### Subversion
Também conhecido como SVN é um outro controlador de versão muito popular, ele é usado nos plugins e repositórios do [WordPress](https://wordpress.org).
