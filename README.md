# Diseño del DAaaS

## Definición de la estrategia del DAaaS 🏗️

Se desarrollará una página web para uso exclusivo y privado de una compañía, con el propósito de clasificar videojuegos. Esta clasificación se llevará a cabo considerando criterios de popularidad y precio. La finalidad de esta plataforma es identificar los artículos de cada categoría – por ejemplo: Xbox, Nintendo, PlayStation, entre otros – que presenten una alta demanda y rentabilidad, con el objetivo de importarlos a Latinoamérica para su posterior comercialización.

La plataforma le permitirá a la compañía acceder a información relevante sobre los artículos, incluyendo: (i) identificación del artículo con base en la región, a saber: Estados Unidos, Europa y Japón; (ii) estado de conservación; (iii) precios de venta; y (iv) las valoraciones cuantitativas de los usuarios y prensa especializada. A continuación se detalla la forma como se visualizará la información:

* ID, Image, Title, Console: Identificación del videojuego por su imagen, nombre, consola/región.  
*	Loose Price: Precio del cartucho o disco del juego por sí solo, no incluye otros elementos.
*	Complete Price (CIB): Precio del juego completo en caja. El cartucho o disco se incluye con la caja original y el manual de instrucciones original.
*	Brand New Price: Precio del juego, la caja y el manual están incluidos y están en su plástico original sellado de fábrica.
*	Graded Price: Precio del artículo nuevo que ha sido calificado por una agencia de calificación de condiciones como VGA o CGC Games.
*	MetaScore y UserScore: Valoraciones cuantitativas promedio de prensa especializada y usuarios.  

Adicionalmente, se realizará el procesamiento de la información de ventas promedio de los artículos en tiendas en línea de Latinoamérica, como MercadoLibre, para realizar comparativas de precios. Se aplicará un tratamiento de datos para representar de manera precisa la oferta en LATAM en este segmento.

De esta manera, la compañía tendrá la capacidad de analizar individualmente cada artículo, lo que permitirá un análisis comparativo más detallado basado en el valor promedio de venta de los artículos a importar. También podrán marcar productos de interés o agregarlos directamente a la lista de artículos a importar.

La información anteriormente descrita se mostrará en una tabla de calificación con la siguiente estructura:


![image](https://github.com/pardo2410/BigDataArchitecture/assets/10873597/e704e4c1-04e8-4868-82dd-4b971ad5b423)


Además, se diseñará una tabla detallada de artículos la cual permitirá visualizar el historial de precios del articulo a lo largo del tiempo. Esta herramienta será valiosa para identificar tendencias de valor y fluctuaciones de precios. A continuación, se muestra la estructura de la tabla detallada por artículo: 

![image](https://github.com/pardo2410/BigDataArchitecture/assets/10873597/7a03c07d-5650-48a0-8b33-ca56161bfe1a)


Es importante mencionar que el dashboard se actualizará automáticamente de forma semanal para teniendo en cuenta la estabilidad de los precios en este segmento. 

A medida que la página web se desarrolle y expanda para satisfacer las necesidades de la empresa, se tienen previstas funcionalidades adicionales, que incluyen:

*	Integración con plataformas de subastas: Esto permitirá a los usuarios rastrear y participar en subastas en línea de artículos coleccionables, además de proporcionar información en tiempo real sobre el estado de las subastas.
*	Comunidad y foros: Se conformará una comunidad en línea donde los coleccionistas podrán discutir, intercambiar información y compartir consejos sobre artículos específicos y tendencias en el mercado.
*	Herramientas de análisis avanzado: Se ofrecerán herramientas analíticas avanzadas que permitirán a los usuarios realizar análisis detallados de datos, como correlaciones entre diferentes factores y pronósticos de precios.
*	Gestión de Inventarios: Se proporcionarán herramientas para que los usuarios puedan llevar un registro de su inventario de artículos a importar, gestionando cantidades y condiciones.

## Arquitectura DAaaS 📚

*	Web page.
*	Base de datos Google Cloud SQL.
*	Crawler mediante Cloud Functions para obtener la información relacionada con valoraciones de prensa (MetaScore) y usuarios (User Score) directamente a la URL de Metacritic.
*	Crawler mediante Cloud Functions para obtener la información del precio de venta de los artículos a analizar a través de la API de MercadoLibre y cálculo del precio promedio por artículo.  
*	Scrapping mediante Cloud Functions para extraer la información de la URL de PriceCharting (Image, Console, Title, Losse Price, Complete Price, New Price, Grade Price).
*	Se utilizará Cloud Functions para efectuar las consultas a los diferentes csv con el fin de generar los reportes que posteriormente consultará el usuario.
*	Implementación de Cloud Schedeler para actualizar los registros de los datos de manera semanal.

## DAaaS Operating Model Design and Rollout 📃 

El primer paso será implementar web Scraping en Python, utilizando las bibliotecas Selenium y BeautifulSoup (BS4), con el fin de extraer información de la URL de PriceCharting. La información a extraer incluye: Image, Title, Console, Losse Price, Complete Price, New Price, Grade Price. Esta información será almacenada en un archivo en formato .csv en una base de datos relacional Google SQL.

El siguiente paso será extraer la información de Metacritic, que incluye el MetaScore y el User Score, para esto es necesario realizar un web Crawler en Python utilizando la biblioteca Scrapy. Este proceso generará una lista de artículos con las puntuaciones promedio de los usuarios y de la prensa especializada, y creará un archivo .csv que se almacenará en Google SQL.

A continuación, se llevará a cabo un segundo web Crawler en Python, empleando las bibliotecas Requests y BS4, con el objetivo de extraer información vinculada a los artículos y sus precios de venta desde la página de MercadoLibre. Asimismo, se realizará el cálculo del valor promedio por articulo utilizando Cloud Function y el resultado se guardará en un archivo .csv que será almacenado en Google SQL.

Por último, se generan la tabla de clasificación y las tablas detalladas por artículo utilizando Cloud Functions para crear los informes que se almacenarán posteriormente en Google SQL. Estos informes podrán ser consultados por el usuario final. Este proceso se actualizará semanalmente mediante la implementación de Cloud Scheduler. 


## Diagrama 🛠️

![image](https://github.com/pardo2410/BigDataArchitecture/assets/10873597/900fb3a6-746d-4bee-ac42-25e0611de763)


