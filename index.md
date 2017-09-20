# Dislexia

> Esta página es la traducción del post [Dsxyliea](http://geon.github.io/programming/2016/03/03/dsxyliea), de [Victor Widell](https://github.com/geon/)

Una amiga que tiene dislexia me describió lo que siente al leer. Ella *puede* leer, pero le toma mucha concentracin, y siente como si las letras "se le movieran" alrededor.

Recuerdo haber leído sobre la [_typoglicemia_](https://en.wikipedia.org/wiki/Typoglycemia). ¿Sería posible hacerlo interactivamente con Javascript? ¡Claro que sí!

¿Ganas de hacer un bookmarklet the esto o algo? [Forkealo](https://github.com/geon/geon.github.com/blob/master/_posts/2016-03-03-dsxyliea.md) en Github.

> La dislexia es la dificultad de aprendizaje que afecta a la lectoescritura, es de carácter específico y persistente. Se da en personas que no presentan ninguna discapacidad física, motriz, visual o de cualquier otro tipo. Asimismo, las personas con dislexia tienen un desarrollo cognitivo normal. Erróneamente el término se aplica a la dificultad para una correcta escritura, en este caso el término médico apropiado es el de disortografía.

> Existen ciertas características que se presentan siempre y que es necesario observar espacialmente:
>
> - Dificultades en el lenguaje escrito
> - Dificultades en la escritura
> - Serias dificultades con la ortografía
> - Lento aprendizaje de la lectura
> - Dificultades para aprender y escribir segundas lenguas.
>
> Por otro lado, a menudo podrán presentarse las siguientes:
>
> - Dificultades en matemáticas
> - Dificultades para seguir instrucciones y secuencias complejas de tareas
> - Problemas de comprensión de textos escritos
> - Fluctuaciones muy significativas de capacidad
>
> A veces, pueden darse según el tipo de dislexia o de como esta haya afectado a la persona:
>
> - Dificultades en el lenguaje hablado
> - Problemas de percepción de las distancias y del espacio
> - Confusión entre derecha e izquierda
> - Problemas con el ritmo y los lenguajes musicales

*Fuente: [Wikipedia](https://es.wikipedia.org/wiki/Dislexia)*




<script type="text/javascript" src="//cdnjs.cloudflare.com/ajax/libs/jquery/2.0.3/jquery.min.js"></script>
<script type="text/javascript">

"use strict";

$(function(){

	var getTextNodesIn = function(el) {
	    return $(el).find(":not(iframe,script)").addBack().contents().filter(function() {
	        return this.nodeType == 3;
	    });
	};

	// var textNodes = getTextNodesIn($("p, h1, h2, h3"));
	var textNodes = getTextNodesIn($("*"));



	function isLetter(char) {
		return /^[\d]$/.test(char);
	}


	var wordsInTextNodes = [];
	for (var i = 0; i < textNodes.length; i++) {
		var node = textNodes[i];

		var words = []

		var re = /\w+/g;
		var match;
		while ((match = re.exec(node.nodeValue)) != null) {

			var word = match[0];
			var position = match.index;

			words.push({
				length: word.length,
				position: position
			});
		}

		wordsInTextNodes[i] = words;
	};


	function messUpWords () {

		for (var i = 0; i < textNodes.length; i++) {

			var node = textNodes[i];

			for (var j = 0; j < wordsInTextNodes[i].length; j++) {

				// Only change a tenth of the words each round.
				if (Math.random() > 1/10) {

					continue;
				}

				var wordMeta = wordsInTextNodes[i][j];

				var word = node.nodeValue.slice(wordMeta.position, wordMeta.position + wordMeta.length);
				var before = node.nodeValue.slice(0, wordMeta.position);
				var after  = node.nodeValue.slice(wordMeta.position + wordMeta.length);

				node.nodeValue = before + messUpWord(word) + after;
			};
		};
	}

	function messUpWord (word) {

		if (word.length < 3) {

			return word;
		}

		return word[0] + messUpMessyPart(word.slice(1, -1)) + word[word.length - 1];
	}

	function messUpMessyPart (messyPart) {

		if (messyPart.length < 2) {

			return messyPart;
		}

		var a, b;
		while (!(a < b)) {

			a = getRandomInt(0, messyPart.length - 1);
			b = getRandomInt(0, messyPart.length - 1);
		}

		return messyPart.slice(0, a) + messyPart[b] + messyPart.slice(a+1, b) + messyPart[a] + messyPart.slice(b+1);
	}

	// From https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/random
	function getRandomInt(min, max) {
		
		return Math.floor(Math.random() * (max - min + 1) + min);
	}


	setInterval(messUpWords, 50);
});


</script>
