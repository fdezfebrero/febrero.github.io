---
title: "Xogos de parexas con Javascript"
layout: post
date: 2021-09-10 01:40
image: /assets/images/js1.jpeg
headerImage: true
tag:
- javascript
- HTML
- CSS
- web
star: true
category: blog
author: javier
description: Xogo de encontrar as parexas, realizado en javascript
---

# Javascript

[Javascript][1] es un lenguaje de programación interpretado, dialecto del estándar ECMAScript. Se define como orientado a objetos,2​ basado en prototipos, imperativo, débilmente tipado y dinámico. 

###  Creación do proxecto

{% highlight html %}

<!DOCTYPE html>
<html lang="es">

<head>
    <meta charset="UTF-8">
    <title>Juego de Memoria</title>
    <link rel="stylesheet" href="style.css"></link>
    <script src="app.js" charset="UTF-8"></script>
</head>

<Body>
    <h1>Juego de memoria</h1>
    <h2>Encuentra la pareja</h2>
    <h3>Resultado:<span id="result"></span></h3>
    <div class="grid">

    </div>
    <footer>
        <p>Javier Fdez &copy; Javiferfeb@live.com</p>
    </footer>
</Body>

</html>

{% endhighlight %}


---

## Configuración do javascript

{% highlight js %}

document.addEventListener('DOMContentLoaded', () => {

    //opciones de cartas
    const cardarray = [{
            name: 'red',
            img: 'img/red.png'
        },
        {
            name: 'red',
            img: 'img/red.png'
        },
        {
            name: 'yellow',
            img: 'img/yellow.png'
        },
        {
            name: 'yellow',
            img: 'img/yellow.png'
        },
        {
            name: 'blue',
            img: 'img/blue.png'
        },
        {
            name: 'blue',
            img: 'img/blue.png'
        },
        {
            name: 'green',
            img: 'img/green.png'
        },
        {
            name: 'green',
            img: 'img/green.png'
        },
        {
            name: 'pink',
            img: 'img/pink.png'
        },
        {
            name: 'pink',
            img: 'img/pink.png'
        },
        {
            name: 'orange',
            img: 'img/orange.png'
        },
        {
            name: 'orange',
            img: 'img/orange.png'
        }
    ]


    // ordena las cartas de forma aleatoria
    cardarray.sort(() => 0.5 - Math.random())



    //variables
    const grid = document.querySelector('.grid')
    const resultdisplay = document.querySelector('#result')

    var cardschosen = []
    var cardschosenid = []
    var cardswon = []

    //crear funcion para crear el grid

    function createboard() {
        for (let i = 0; i < cardarray.length; i++) {
            var card = document.createElement('img')
            card.setAttribute('src', 'img/blank.png')
            card.setAttribute('data-id', i)
            card.addEventListener('click', flipcard)
            grid.appendChild(card)
        }
    }

    //comprobar si coinciden

    function checkformatch() {
        var cards = document.querySelectorAll('img')
        const optiononeid = cardschosenid[0]
        const optiontwoid = cardschosenid[1]

        if (cardschosen[0] === cardschosen[1]) {
            // alert('Vamos, encontraste una pareja')
            cards[optiononeid].setAttribute('src', 'img/blanco.png')
            cards[optiontwoid].setAttribute('src', 'img/blanco.png')
            cardswon.push(cardschosen)
        } else {
            cards[optiononeid].setAttribute('src', 'img/blank.png')
            cards[optiontwoid].setAttribute('src', 'img/blank.png')
                // alert('Lo siento, vuelve intentarlo')
        }

        cardschosen = []
        cardschosenid = []
        resultdisplay.textContent = cardswon.length
        if (cardswon.length === cardarray.length / 2) {
            resultdisplay.textContent = 'Felicidades, las encontraste todas'

        }

    }


    //girar carta
    function flipcard() {
        var cardid = this.getAttribute('data-id')
        cardschosen.push(cardarray[cardid].name)
        cardschosenid.push(cardid)
        this.setAttribute('src', cardarray[cardid].img)

        if (cardschosen.length === 2) {
            setTimeout(checkformatch, 500)
        }
    }

    createboard()


})

{% endhighlight %}

## Resultado

![origenfriki][6]

![origenfriki][7]


### Docs
Para mais informacion [kubowania][3]

O meu [repo][4]

A paxina do [Xogo][5]



[1]: https://www.javascript.com/
[2]: https://www.youtube.com/watch?v=lhNdUVh3qCc
[3]: https://github.com/kubowania/memory-game
[4]: https://github.com/javierfdezfebrero/javierfdezfebrero
[5]: origenfriki.es
[6]: https://i.imgur.com/4k3kBYA.png
[7]: https://i.imgur.com/mwi3MRx.png
