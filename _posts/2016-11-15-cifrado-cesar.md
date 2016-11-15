---
layout: post
title: "Cifrado César"
date: 2016-11-15 10:26:04 -0500
categories: crypto
---

En criptografía, el cifrado César, también conocido como cifrado por desplazamiento, es una de las técnicas de cifrado más simples y más usadas. Es un tipo de cifrado por sustitución en el que una letra en el texto original es reemplazada por otra letra que se encuentra un número fijo de posiciones más adelante en el alfabeto. [wikipedia](https://es.wikipedia.org/wiki/Cifrado_C%C3%A9sar)

<br>
![](https://upload.wikimedia.org/wikipedia/commons/thumb/2/2b/Caesar3.svg/640px-Caesar3.svg.png)
<br>

Por ejemplo, con un desplazamiento de 3, la A sería sustituida por la D (situada 3 lugares a la derecha de la A), la B sería reemplazada por la E, etc. Este método debe su nombre al dictador romano Julio César, quien lo usaba para comunicarse con sus generales.

## Cifrado

Para implementar el cifrado César en Ruby tomamos el texto claro y a cada uno de sus caracteres le sumamos el desplazamiento.

    #!/usr/bin/ruby

    texto_claro = "Este es el MENSAJE SECRETO"
    desplazamiento = 3

    caracteres_cifrados = []
    texto_claro.each_char do |c|
      caracteres_cifrados << (c.ord + desplazamiento).chr
    end

    texto_cifrado = caracteres_cifrados.join('').to_s

    p texto_cifrado

## Descifrado

El proceso de descifrado se realiza restándole el desplazamiento a cada uno de los caracteres del texto cifrado.

    #!/usr/bin/ruby

    texto_cifrado = "Hvwh#hv#ho#PHQVDMH#VHFUHWR"
    desplazamiento = 3

    caracteres_descifrados = []
    texto_cifrado.each_char do |c|
      caracteres_descifrados << (c.ord - desplazamiento).chr
    end

    texto_descifrado = caracteres_descifrados.join('').to_s

    p texto_descifrado

## Ataques

Existen dos tipos de ataques para descifrar un mensaje que ha sido encriptado con el cifrado César:

### Fuerza bruta

Este ataque consiste en probar uno a uno todos los desplazamientos posibles hasta encontrar un mensaje coherente.

    #!/usr/bin/ruby

    texto_cifrado = "Hvwh#hv#ho#PHQVDMH#VHFUHWR"
    desplazamiento_maximo = 36

    desplazamiento_maximo.times do |i|
      caracteres_descifrados = []
      texto_cifrado.each_char { |c| caracteres_descifrados << (c.ord - i).chr }
      texto_descifrado = caracteres_descifrados.join('').to_s
      p "Desplazamiento: #{i} -> #{texto_descifrado}"
    end

### Análisis de frecuencia

En este ataque se identifican las letras más comunes en el mensaje cifrado, y conociendo la distribución de letras del idioma original del mensaje original, se puede determinar fácilmente el valor del desplazamiento. Por ejemplo, en español, las letras más frecuentes son la E y la A, mientras que las menos frecuentes son la K y la W.

    #!/usr/bin/ruby

    texto_cifrado = "Hvwh#hv#ho#PHQVDMH#VHFUHWR"

    frecuencias = {}

    texto_cifrado.each_char do |c|
      frecuencias[c] = 0 unless frecuencias.include? c
      frecuencias[c] += 1
    end

    caracteres_mas_frecuentes = frecuencias.sort_by{|k,v| v}.reverse.take(3).to_h
    caracteres_mas_frecuentes.each do |f|
      desplazamiento = f[0].ord - "E".ord
      puts "\nSuponiendo #{f[0]} -> E, desplazamiento: #{desplazamiento}"
      caracteres_descifrados = []
      texto_cifrado.each_char { |c| caracteres_descifrados << (c.ord - desplazamiento).chr }
      texto_descifrado = caracteres_descifrados.join('').to_s
      puts texto_descifrado
    end

