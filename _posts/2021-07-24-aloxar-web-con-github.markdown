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

# GitHub

[Github][1] e unha plataforma de desarrollo colaborativo para aloxar proxectos usando o sistema de control de versions Git. Utilizase principalmente pra creación de código fuente de programas de ordenador. 

---

###  Creamos un repo desde GitHub

![Markdowm Image][2]
<figcaption class="caption">Novo repo desde Github</figcaption>

##### En GitHub debemmos gardar o proxecto con nome do noso proxecto e con .github.io o final (XXXXXX.github.io)

---

###  Creamos un repo desde o linea de comandos

{% highlight bash %}
echo "# novo_repo" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
{% endhighlight %}

---

###  Añadimos un repo remoto e facemos Puch para subir os arquivos locales

{% highlight bash %}
git remote add origin https://github.com/fdezfebrero/novo_repo.git
git push -u origin main
{% endhighlight %}

---
###  Debemos seleccionar no Source a Rama que queiramos publicar

![Markdowm Image][3]
<figcaption class="caption">Pages Github</figcaption>

---

###  En custom domain añadimos o noso dominio personalizado

![Markdowm Image][4]
<figcaption class="caption">Pages Github</figcaption>

---
###  Publicar a nosa páxina aloxada en GitHub cun dominio

Debemos configurar o DNS no noso proveedor de dominio

Configuramos os registros ALIAS, ANAME, ou A como:

{% highlight bash %}

185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153

Exemplo:

> EXAMPLE.COM     3600    IN A     185.199.108.153
> EXAMPLE.COM     3600    IN A     185.199.109.153
> EXAMPLE.COM     3600    IN A     185.199.110.153
> EXAMPLE.COM     3600    IN A     185.199.111.153

Añadimos un CNAME con nome do domino de Github

> xxxx.github.io              3592    IN      CNAME 

{% endhighlight %}

---



[1]: https://github.com/
[2]: https://i.imgur.com/HpKEj2g.png
[3]: https://i.imgur.com/kTnT2n1.png
[4]: https://i.imgur.com/EfcjVhz.png



