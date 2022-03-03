# CHATBOT - Python - Facebook Messenger 🤖

_Estre proyecto consiste en le desarrollo de un chatbot utilizando como lenguaje de programación Python versión 2.7 para la plataforma de Facebook Messenger y almacenado en la nube de Google (Google Cloud)._

<br>

Para configurar tu entorno de desarrollo en Python 2, debes hacer lo siguiente:

1. Instala la versión más reciente de Python 2.

2. Consulta Entorno de ejecución de Python 2 para obtener una lista de las versiones compatibles.

3. Instala e inicializa la CLI de gcloud para implementar y administrar tus apps. Si ya instalaste e inicializaste la CLI de gcloud, ejecuta el comando gcloud components update para actualizarla a la versión más reciente.

**Instala el componente de gcloud, que incluye la extensión de App Engine para Python 2.**
<br>
<br>

Utiliza el siguiente comando para instalar los componentes desde la CLI de gcloud:
<br>

```
gcloud components install app-engine-python
```
<br>

Una vez instalada la CLI de Gcloud y los componentes necesarios para el entorno de desarrollo en Python 2, lo que sigue es crear una aplicación con App Engine.
<br>

![App Engine](https://raw.githubusercontent.com/EliasBautista/chatbot-fb-messenger/master/Img/1.jpg)
<br><br>

Para desarrollar un Chatbot en la plataforma de Messenger necesitas una cuenta en Facebook for Developers así como también una página en Facebook que este dada de alta como empresa/negocio y cumpla con los requisitos que solicita Facebook para validarla.
<br>

En la sección de configuración de Messenger en el apartado de Weebhooks, podemos encontrar los campos de subscripción que nos interesan para el chatbot, en este caso "messages" y "messaging_postbacks" los postbacks son para saber que botones presiono el usuario y en base a eso retornar una respuesta.
<br>

![Messenger](https://raw.githubusercontent.com/EliasBautista/chatbot-fb-messenger/master/Img/2.jpg)


En esta sección también se solicita una URL de la devolución de la llamada la cuál sera la URL donde se hizo deploy nuestra app de google cloud. Y también una cadena de texto para verificar un token, puede ser cualquier cadena, mientras no se comparta con nadie. En este caso será "facebook_verification_token".
<br><br>

![Messenger](https://raw.githubusercontent.com/EliasBautista/chatbot-fb-messenger/master/Img/3.jpg)

Antes de poder confirmar los weebhooks se necesita validar la URL de la llamada, como se muestra en el siguiente pedazo de código:

```
VERIFY_TOKEN = "facebook_token_verificacion"

class MainPage(webapp2.RequestHandler):
	def get(self):
			self.response.headers['Content-Type'] = 'text/plain'
			mode = self.request.get("hub.mode")
			if mode == "subscribe":
				challenge = self.request.get("hub.challenge")
				verify_token = self.request.get("hub.verify_token")
				if verify_token == VERIFY_TOKEN:
					self.response.write(challenge)
			else:
				self.response.write("Ok, Elias Bautista")
```
Lo primero que hacemos es definir un content type, un texto plano, en la linea 21 obtenemos un parametro get de la petición, estamos obteniendo el parametro hub.mode, de manera que si es igual a subscribe obtenemos la variable challenge y el token de verificación, si el token que envía Facebook es igual al que definimos respondemos con el valor de challenge.

Una vez definido lo anterior podemos hacer deploy de nuevo a la aplicación y verificar la URL en Facebook.

![Messenger](https://raw.githubusercontent.com/EliasBautista/chatbot-fb-messenger/master/Img/4.jpg)

**Se debe tener especial cuidado con no mezclar espacios y tabs al escribir este código en Python.**

El archivo **Chatbot tree.yaml** contiene todas las posibles respuestas que puede tener el usuario, acorde a las selecciones que haga.