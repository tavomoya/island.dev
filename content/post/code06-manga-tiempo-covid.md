+++
title = "Code 06 - Manga en Tiempos de COVID"
tags = [
	"code",
	"work",
	"git",
	"manga",
	"go",
	"telegram",
	"bot",
	"mangagram",
]
date = "2020-06-12"
menu = "main"
+++

Como a todos durante esta cuarentena a mi también me ha atacado un poco el aburrimiento, y qué mejor uso para este tiempo de ocio que un poco de práctica con algo nuevo, por lo menos para mi. 

Hace un tiempo en un grupo en Telegram con unos amigos surgió la idea de crear un bot que nos alertara cuando hubiese un nuevo capítulo disponible de nuestros mangas favoritos, lo sé lo sé muy nerd, pero todos los del grupo somos fan de diversos mangas y no tener que estar pendiente cada día a las diferentes páginas en las que salen presenta una ventaja para nosotros. 

A partir de eso empecé a trabajar en MangaGram (si, ya está disponible como bot en Telegram). Las funcionalidades que tiene inicialmente son las siguientes:

 * Poder “suscribirme” a un manga
 * Poder “suscribirme” a diferentes fuentes de manga (paginas que publican)
 * El bot se encargaría de mandar un mensaje al chat (individual o a un grupo) de que un nuevo capítulo esta disponible del manga (o los mangas) al que ese chat se suscribió.

Aparenta ser bastante sencillo, sin embargo, nunca había trabajado con ningún bot, ni con la API de Telegram. Además de que casi todos los recursos que encontraba recomendaban hacerlo con Python, yo quería hacerlo con Nodejs o Go, así que ya esto presentaba como tal un “reto”.

## El Proceso

Terminé decidiendo por trabajarlo en Go, mi primera tarea fue buscar si ya existía algún wrapper en Go del API de Telegram que me facilite un poco más el trabajo de lidiar directamente con dicho API. Para mi fortuna, encontré varios pero me quede con este [Telebot](https://github.com/tucnak/telebot), porque había estado mantenido recientemente ademas de que la documentación me pareció bastante friendly.

Viendo como funcionan los bots, lo único que necesitaba era crear una especie de servidor web en el que en vez de exponer endpoints, iba a exponer cuáles eran los comandos que desencadenan acciones en el bot. 

El primer paso hacia crear el bot es bastante simple, no tiene nada que ver con codigo. En Telegram existe un bot llamado BotFather, este es el que se encarga de administrar los bots de todo el mundo, aquí creas y editas tu bot. BotFather te provee de un key único que utilizas en tu programa para que el API de Telegram pueda identificar a qué bot pertenece. (Ten pendiente que el key no debe estar publicado en ninguna parte ya que es información sensible de tu bot).

Una vez con el key decidí comenzar creando la parte principal del bot:

```go
func main() {
	log.Println("Started Manga Gram bot")

	port := os.Getenv("PORT")
	publicURL := os.Getenv("PUBLIC_URL")
	token := os.Getenv("TOKEN")

	if port == "" {
		port = "9000"
	}

	listen := fmt.Sprintf(":%s", port)

	webhook := &tb.Webhook{
		Listen:   listen,
		Endpoint: &tb.WebhookEndpoint{PublicURL: publicURL},
	}

	settings := tb.Settings{
		Token:  token,
		Poller: webhook,
	}

	bot, err := tb.NewBot(settings)
	if err != nil {
		log.Fatal("there was an error creating the bot: ", err)
	}
	
	...

}
```

Aquí utilizo el paquete **os** de Go para leer algunas variables de entorno. Token se refiere al key único que nos dio BotFather, PUBLIC_URL es la URL a la que el API de telegram va a redireccionar los mensajes del Bot y PORT pues es el puerto en el que está corriendo mi aplicacion, la cual si no es especificado corre el puerto 9000 por defecto.

Estos son los únicos pasos necesarios para crear el bot. Ahora bien, faltan los comandos, pero como dije anteriormente es bastante similar a configurar un servicio web, la única diferencia es que los endpoints son comandos:

```go
bot.Handle("/start", func(m *tb.Message) {
	// some code
})

bot.Handle("/manga", func(m *tb.Message) {
	// some code
})
```

Estos dos comandos lo que indican es que cuando desde Telegram se use “/start” o “/manga” el bot hará lo que sea que esté dentro de estas funciones.

Para manejar el tema de las diferentes fuentes o páginas de manga, opté por crear una interfaz con ciertos métodos definidos que se compartan entre las diferentes fuentes. Quedé con algo como esto:

```go
// MangaFeedInterface defines the interface to all
// methods in the different manga sources.
type MangaFeedInterface interface {
	QueryManga(string) *models.ApiQuerySuggestions
	ViewManga() string
	Subscribe(subscription *models.Subscription) error
	GetLastMangaChapter(string) (string, error)
}
```

En donde **QueryManga** como su nombre lo indica se usa para buscar un manga específico en esa fuente, **ViewManga** devuelve la URL de ese manga en esa página, **Subscribe** te suscribe a ese manga para esa página y **GetLastMangaChapter** busca el último capítulo publicado en esta página.

Ahora bien, quizás se pregunten cómo consigo buscar mangas en diferentes páginas, no es como que todas tienen un API unificado, cada una tiene una forma diferente de hacer esto. En algunas si hay un servicio rest al que simplemente hago requests, en otras debo valerme de hacer scraping de la página. 

## Ya podemos buscar, bien, pero cómo te suscribes?

Bueno, para suscribir usuarios debía guardar algunas informaciones en algún lado y no quería tener que configurar algo super elaborado en Postgres o MySQL para esto, así que me decidí por utilizar MongoDB. Para suscripción lo que necesito saber es, el ID del chat de Telegram que se suscribió, cual es el nombre del manga, en que pagina esta y como llego a el. 

Creé una base de datos en mLab (gratis) con una colección de “subscriptions”, aquí guardaría todos los objetos de suscripción. 

Aquí también entra el método que vimos anteriormente llamado **GetLastMangaChapter**. El bot necesita una forma de saber cual es el último capítulo para ese manga en esa página, sin esta información no tiene forma de saber cuando hay un capítulo nuevo o no. Así que cada vez que un usuario se suscribe a un manga, a ese objeto también se le agrega el ultimo capitulo disponible. 

```go
// Subscribe method receives a subscription model, this contains information about
// a User or Group that wants to receive alerts from a certain Manga title.
// The method will save this in a 'Subscription' collection in MongoDB, as well as
// set a value for the lastChapter of the Manga title.
func (m *Manganelo) Subscribe(subscription *models.Subscription) error {

	// Validate subscription data
	if subscription.MangaName == "" || subscription.MangaURL == "" {
		log.Println("No manga supplied for subscription")
		return errors.New("No manga supplied for subscription")
	}

	if subscription.ChatID == 0 {
		log.Println("No Chat supplied for subscription")
		return errors.New("No Chat supplied for subscription")
	}

	subscription.ID = primitive.NewObjectID()
	subscription.MangaFeed = 2

	subscription.LastChapterURL, _ = m.GetLastMangaChapter(subscription.MangaURL)

	_, err := m.DB.MongoClient.Collection("subscription").InsertOne(m.DB.Ctx, subscription)
	if err != nil && !strings.Contains(err.Error(), "subscription_unq") {
		log.Println("There was an error creating new subscription: ", err)
		return err
	}

	return nil
}
```

## Todo eso está muy bien, pero cómo el bot sabrá que hay un capítulo nuevo?

Lo primero que se me ocurrió para que el bot se mantenga revisando las suscripciones fue crear un cron job que lea las suscripciones, vea cual es el último capítulo para cada manga, revise su el último capítulo encontrado es igual al que está guardado, y si no son iguales pues que mande una notificación.

Esta forma de hacerlo no es perfecta para nada, y al igual que otros métodos usados aquí (scraping) está sujeta a factores que no puedo controlar muy específicos de cada fuente o página. 

#### Ejemplo de búsqueda del último cap. 
```go
// GetLastMangaChapter method receives the URL to a manga title and returns
// the last chapter published in this URL. An error might be returned if
// no URL is supplied or if it cannot connect to the URL
func (m *Manganelo) GetLastMangaChapter(titleURL string) (string, error) {

	if titleURL == "" {
		log.Println("No title supplied")
		return "", nil
	}

	page, err := goquery.NewDocument(titleURL)
	if err != nil {
		log.Println("There was an error getting the page: ", err)
		return "", err
	}

	lastChaperUrl, _ := page.Find("a.chapter-name").First().Attr("href")

	return lastChaperUrl, nil
}
```

#### Cron Job

```go
// GetMangaUpdates function runs a CRON job every 12h.
// The job queries the subscription collection and looks
// for new chapters. If a new chapter is found, a message
// is sent to the Chat that got subscribed to the title.
func GetMangaUpdates(job *models.Job, bot *tb.Bot) {
	job.Cron.AddFunc(schedule, func() {
		jobName := "GetMangaUpdates"
		log.Println("Running Manga Updates CRON")
		started := time.Now()
		fmt.Println("*** [*] CRON job 'GetMangaUpdates' started ***")
		fmt.Printf("*** [*] CRON job 'GetMangaUpdates' start time: %v ***\n", started)

		cursor, err := job.DB.MongoClient.Collection("subscription").Find(job.DB.Ctx, bson.M{})
		if err != nil {
			onError(jobName, started, err)
		}

		subs := make([]*models.Subscription, 0)

		err = cursor.All(job.DB.Ctx, &subs)
		if err != nil {
			onError(jobName, started, err)
		}

		for _, manga := range subs {
			feed := NewMangaInterface(manga.MangaFeed, job.DB)
			if feed == nil {
				continue
			}

			// Get the last chapter for each manga
			last, err := feed.GetLastMangaChapter(manga.MangaURL)
			if err != nil || last == "" {
				continue // LAter will decide what to do here
			}

			if manga.LastChapterURL == "" || last != manga.LastChapterURL {
				manga.LastChapterURL = last
				msg := fmt.Sprintf("Here is a new chapter for %s\n %s", manga.MangaName, last)
				to, _ := bot.ChatByID(strconv.FormatInt(manga.ChatID, 10))
				bot.Send(to, msg)
				updateLastChapter(manga, job)
			}
		}

		onSuccess(jobName, started)
	})

	job.Cron.Start()
}
```

## Conclusiones

Ha sido bastante divertido trabajar en esto, me mantuvo entretenido en estos tiempos de confinamiento y actualmente es una herramienta que sigo utilizando. El desarrollo en esta no ha concluido, aún tengo en mente agregar algunas fuentes en otros idiomas, y mejorar algunas de las funcionalidades. 

Si este proyecto te parece interesante y quieres contribuir estoy abierto a recibir PR, sugerencias y recomendaciones en el [repo en GitHub](https://github.com/tavomoya/mangagram). Si te interesa utilizarlo puedes buscar [MangaGram](http://t.me/MangaGramBot) en Telegram. 



