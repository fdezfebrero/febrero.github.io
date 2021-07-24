---
title: "Aloxar web con Github"
layout: post
date: 2021-07-24 01:40
image: /assets/images/github-logo.jpeg
headerImage: true
tag:
- markdown
- GitLab
- GitHub
- web
star: true
category: blog
author: febrero
description:  Como crear unha web estatica con Jekyll
---

# Jekylls

[Jekyll][1] é un xenerador simple pra **sitios web estáticos con capacidades de blog**; adecuados pra sitios web persoales, de proxecto ou de organizacións. Distribuese baixo a *licenza de Código aberto MIT*.

###  Instalación e creación do primer proxecto
{% highlight bash %}
$ gem install bundler jekyll
$ jekyll new o-meu-sitio-web
$ cd o-meu-sitio-web
{% endhighlight %}


---

## Configuración do tema por defecto

Para configurar o os valores usase o ficheiro _config.yml

Modificanse os parametros como Title, email, description... para que se modifiquen no index que se mostrara.

{% highlight markdown %}
 Welcome to Jekyll!

This config file is meant for settings that affect your whole blog, values
 which you are expected to set up once and rarely edit after that. If you find
 yourself editing this file very often, consider using Jekyll's data files
 feature for the data you need to update frequently.

 For technical reasons, this file is *NOT* reloaded automatically when you use
 'bundle exec jekyll serve'. If you change this file, please restart the server process.

 If you need help with YAML syntax, here are some quick references for you: 
https://learn-the-web.algonquindesign.ca/topics/markdown-yaml-cheat-sheet/#yaml
 https://learnxinyminutes.com/docs/yaml/

 Site settings
 These are used to personalize your new site. If you look in the HTML files,
 you will see them accessed via {{ site.title }}, {{ site.}}, and so on.
 You can create any custom variable you would like, and they will be accessible
 in the templates via {{ site.myvariable }}.

`title:` Your awesome title
`email:` your-email@example.com
`description:` >- # this means to ignore newlines until "baseurl:"
  Write an awesome description for your new site here. You can edit this
  line in _config.yml. It will appear in your document head meta (for
  Google search results) and in your feed.xml site description.
`baseurl:` "" # the subpath of your site, e.g. /blog
`url:` "" # the base hostname & protocol for your site, e.g. http://example.com
`twitter_username:` jekyllrb
`github_username: ` jekyll

 Build settings
theme: minima
plugins:
  - jekyll-feed

 Exclude from processing.
 The following items will not be processed, by default.
 Any item listed under the `exclude:` key here will be automatically added to
 the internal "default list".

 Excluded items can be processed by explicitly listing the directories or
 their entries' file path in the `include:` list.

 exclude:
  - .sass-cache/
  - .jekyll-cache/
  - gemfiles/
  - Gemfile
  - Gemfile.lock
  - node_modules/
  - vendor/bundle/
  - vendor/cache/
  - vendor/gems/
  - vendor/ruby/


{% endhighlight %}


###  Habilitamos o páxina web no servidor local
{% highlight bash %}

$ cd o-meu-sitio-web
/o-meu-sitio-web $ bundle exec jekyll serve

{% endhighlight %}

A páxina web levantase no porto 4000.

No navegador web buscase [http://localhost:4000](http://localhost:4000)


### Podemos descargar temas xa creados pola comunidade


* [GitHub.com #jekyll-theme repos][2]
* [jamstackthemes.dev][3]
* [jekyllthemes.org][4]
* [jekyllthemes.io][5]

### Podemos descargar plugins

* [jekyll-plugin topic on GitHub][6]
* [Planet Jekyll][7]


### Docs
Para mais informacion sobre configuración e uso [Jekyll docs][8]




[1]: https://jekyllrb.com/
[2]: https://github.com/topics/jekyll-theme
[3]: https://jamstackthemes.dev/ssg/jekyll/
[4]: http://jekyllthemes.org/
[5]: jekyllthemes.io
[6]: https://github.com/topics/jekyll-plugin
[7]: https://github.com/planetjekyll/awesome-jekyll-plugins
[8]: https://jekyllrb.com/docs/
