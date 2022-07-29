---
layout: "../../layouts/PostLayout.astro"
setup: |
  import Image from '../../components/Image.astro'
  import Title from '../../components/Title.astro'
title: "Princios Solid en React JS Parte 1 (SRP)"
author: "Lucas Angelino"
date: "1 de Julio de 2022"
readtime: "10"
thumnail: "/img/solid_en_react_1.webp"
description: "En este primer articulo de la saga vas a descubrir los conceptos de SOLID en React"
---

<Image src="/img/solid_en_react_1.webp" alt="Img" />

## Motivación

A medida que la industria del software crece y comete errores, las mejores prácticas y los buenos principios de diseño de software surgen y se conceptualizan para evitar repetir los mismos errores en el futuro. El mundo de la programación orientada a objetos (OOP) en particular es una mina de oro de tales mejores prácticas, y SOLID es, sin duda, uno de los más influyentes.

<br>

SOLID es un acrónimo, donde cada letra representa uno de los cinco principios de diseño que son:
<br>
Principio de responsabilidad única (SRP)
Principio abierto-cerrado (OCP)
Principio de sustitución de L iskov (LSP)
Principio de segregación de interfaz (ISP )
Principio de inversión de dependencia (DIP)
<br>
En este artículo, hablaremos sobre la importancia de cada principio y veremos cómo podemos aplicar los aprendizajes de SOLID en aplicaciones React.
<br>
Sin embargo, antes de comenzar, hay una gran advertencia . Los principios de _SOLID_ se concibieron y delinearon teniendo en cuenta el lenguaje de programación orientado a objetos. Estos principios y su explicación se basan en gran medida en conceptos de clases e interfaces, mientras que JS en realidad no tiene ninguno de los dos. Lo que a menudo consideramos como "clases" en JS son simplemente clases similares simuladas usando su sistema prototipo, y las interfaces no son parte del lenguaje en absoluto (aunque la adición de TypeScript ayuda un poco). Aún más, la forma en que escribimos el código React moderno está lejos de estar orientado a objetos; en todo caso, se siente más funcional.
<br>
Sin embargo, la buena noticia es que los principios de diseño de software como _SOLID_ son independientes del lenguaje y tienen un alto nivel de abstracción, lo que significa que si entrecerramos los ojos lo suficiente y nos tomamos algunas libertades con la interpretación, podremos aplicarlos a nuestro código React más funcional. .
<br>
Así que tomémonos algunas libertades.
<br>

## Principio de responsabilidad única (SRP)

<br>
La definición original establece que _"cada clase debe tener una sola responsabilidad"_, es decir, hacer exactamente una cosa. Este principio es el más fácil de interpretar, ya que simplemente podemos extrapolar la definición a “cada función/módulo/componente debe hacer exactamente una cosa”.
<br>
De los cinco principios, _SRP_ es el más fácil de seguir, pero también es el más impactante, ya que mejora drásticamente la calidad de nuestro código. Para garantizar que nuestros componentes hagan una cosa, podemos:
<br>
dividir componentes grandes que hacen demasiado en componentes más pequeños
extraer código no relacionado con la funcionalidad del componente principal en funciones de utilidad separadas
encapsular la funcionalidad conectada en ganchos personalizados
<br>
Ahora veamos cómo podemos aplicar este principio. Comenzaremos considerando el siguiente componente de ejemplo que muestra una lista de usuarios activos:

<Image src="/img/solid_en_react_SRP_1.svg" alt="solid_en_react_SRP_1 descripcion del problema" />

Aunque este componente es relativamente corto ahora, ya está haciendo bastantes cosas: obtiene datos, los filtra, representa el componente en sí mismo, así como elementos de lista individuales. Veamos cómo podemos descomponerlo.
<br>
En primer lugar, siempre que tengamos enlaces conectados **useState**, **useEffectes** una buena oportunidad para extraerlos en un enlace personalizado:

<Image src="/img/solid_en_react_SRP_2.svg" alt="solid_en_react_SRP_1 descripcion del problema" />

Ahora nuestro **useUsersenlace** se refiere a una sola cosa: obtener usuarios de la API. También hizo que nuestro componente principal fuera más legible, no solo porque se hizo más corto, sino también porque reemplazamos los ganchos estructurales que necesitabas para descifrar el propósito con un gancho de dominio cuyo propósito es inmediatamente obvio por su nombre.
<br>
A continuación, veamos el _JSX_ que representa nuestro componente. Siempre que tengamos un mapeo de bucle sobre una matriz de objetos, debemos prestar atención a la complejidad de _JSX_ que produce para elementos de matriz individuales. Si es una sola línea que no tiene ningún controlador de eventos adjunto, está bien mantenerlo en línea, pero para un marcado más complejo, podría ser una buena idea extraerlo en un componente separado:

<Image src="/img/solid_en_react_SRP_3.svg" alt="solid_en_react_SRP_1 descripcion del problema" />

Al igual que con un cambio anterior, hicimos que nuestro componente principal fuera más pequeño y más legible al extraer la lógica para representar los elementos del usuario en un componente separado.
<br>
Finalmente, tenemos la lógica para filtrar usuarios inactivos de la lista de todos los usuarios que obtenemos de una _API_. Esta lógica está relativamente aislada y podría reutilizarse en otras partes de la aplicación, por lo que podemos extraerla fácilmente en una función de utilidad:

<Image src="/img/solid_en_react_SRP_4.svg" alt="solid_en_react_SRP_1 descripcion del problema" />

En este punto, nuestro componente principal es lo suficientemente corto y sencillo como para dejar de desglosarlo y darlo por terminado. Sin embargo, si miramos un poco más de cerca, notaremos que todavía está haciendo más de lo que debería. Actualmente, nuestro componente está obteniendo datos y luego aplicándoles filtros, pero idealmente, solo queremos obtener los datos y representarlos, sin ninguna manipulación adicional. Entonces, como última mejora, podemos encapsular esta lógica en un nuevo gancho personalizado:

<Image src="/img/solid_en_react_SRP_5.svg" alt="solid_en_react_SRP_1 descripcion del problema" />

Aquí creamos _useActiveUsersun_ gancho para encargarnos de la lógica de búsqueda y filtrado **(también memorizamos datos filtrados para obtener buenas medidas)**, mientras que nuestro componente principal se deja hacer lo mínimo: representar los datos que obtiene del gancho.
<br>
Ahora, dependiendo de nuestra interpretación de "una cosa", podemos argumentar que el componente primero obtiene los datos y luego los procesa, lo cual no es "una cosa". Podríamos dividirlo aún más, llamando a un gancho en un componente y luego pasando el resultado a otro como accesorios, pero encontré muy pocos casos en los que esto sea realmente beneficioso en aplicaciones del mundo real, así que seamos indulgentes con la definición y aceptemos "renderizar los datos que obtiene el componente" como "una cosa".
<br><br>
_Para resumir, siguiendo el principio de responsabilidad única, efectivamente tomamos una gran pieza de código monolítica y la hacemos más modular. La modularidad es excelente porque hace que nuestro código sea más fácil de razonar, los módulos más pequeños son más fáciles de probar y modificar, es menos probable que introduzcamos la duplicación de código no intencional y, como resultado, nuestro código se vuelve más fácil de mantener._
<br>
Debe decirse que lo que hemos visto aquí es un ejemplo artificial, y en sus propios componentes puede encontrar que las dependencias entre las diferentes partes móviles están mucho más entrelazadas. En muchos casos, esto podría ser una indicación de elecciones de diseño deficientes: uso de malas abstracciones, creación de componentes universales que lo hacen todo, alcance incorrecto de los datos, etc., y por lo tanto se puede desenredar con una refactorización más amplia.
<br>
