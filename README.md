# Rastreador de Carga

##### Este projeto é baseado no original [Eclipse Cargo Tracker - Applied Domain-Driven Design Blueprints for Jakarta EE](https://github.com/eclipse-ee4j/cargotracker)

O projeto demonstra como você pode desenvolver aplicações com *Jakarta EE* utilizando as melhores práticas arquiteturais amplamente adotadas, como Domain-Driven Design (DDD). 

A aplicação é um sistema de fim-a-fim para manter o controle da cargas marítimas. 
Ele possui várias interfaces descritas nas seções seguintes.
Para mais detalhes sobre o projeto, por favor, visite: https://eclipse-ee4j.github.io/cargotracker/.

A slide deck introducing the fundamentals of the project is available on the official Eclipse 
Foundation [Jakarta EE SlideShare account](https://www.slideshare.net/Jakarta_EE/applied-domaindriven-design-blueprints-for-jakarta-ee). A recording of the slide deck is available on the official [Jakarta EE YouTube account](https://www.youtube.com/watch?v=pKmmZd-3mhA).

![Cargo Tracker cover](cargo_tracker_cover.png)
 
 ## Começando

O [site do projeto](https://eclipse-ee4j.github.io/cargotracker/) tem informações detalhadas sobre como começar.

Os passos mais simples são os seguintes (sem necessidade de IDE):

* Obter o código fonte do projeto.
* Certifique-se de estar executando Java SE 8, Java SE 11 ou Java SE 17.
* Certifique-se de que o JAVA_HOME está configurado.
* Desde que você tenha a Maven configurada corretamente, navegue até a raiz do código-fonte do projeto e 
  tipo: `mvn clean package cargo:run`
* Vá para http://localhost:8080/cargo-tracker

Para executar o projeto no Eclipse, siga estes passos:

* Configurar Java SE 8, Java SE 11 ou Java SE 17, [Eclipse for Enterprise Java Developers](https://www.eclipse.org/downloads/packages/) e [Payara 5](https://www.payara.fish/downloads/). Você também precisará configurar [Payara Tools](https://marketplace.eclipse.org/content/payara-tools) no Eclipse.
* Importar este código no Eclipse como um projeto Maven, 
  Eclipse fará o resto por você. Proceda com o clean/building da aplicação.
* Depois que o projeto for construído (o que levará algum tempo, pois a Maven baixa pela primeira vez todas as dependências), simplesmente execute-o através do Payara 5.

## Explorando a aplicação

Após a execução do aplicativo, ele estará disponível em: 
http://localhost:8080/cargo-tracker/. Por baixo dos panos, a aplicação utiliza um 
número de funcionalidades do Jakarta EE, incluindo: Faces, CDI, Enterprise Beans, Persistence, REST, Batch, JSON Binding, Bean Validation and Messaging.

Há várias interfaces web, REST e até uma interface para escanear o sistema de arquivos. 
Provavelmente, é melhor começar a explorar as interfaces seguindo a ordem abaixo.

A **interface de rastreamento** permite que você acompanhe o status da **carga** (cargo) e é
destinado ao público em geral. Tente inserir uma identificação de **rastreamento** (tracking) como ABC123 (a 
aplicação é pré-carregada com alguns dados de amostra (ou sample data) ).

A **interface administrativa** é destinada à **empresa de navegação**(shipping company) que administra
carga. A landing page, desta interface, é um _dashboard_ que fornece uma 
visão da carga registrada. Você pode reservar a carga usando a **interface de reserva**.
Uma vez que a carga é **reservada** (booked), você pode **encaminhar** (route) a carga. Quando você inicia um **pedido de roteamento** (routing request),
o sistema determinará as **rotas** (routes) que podem funcionar para a carga. Uma vez que você selecione
uma rota, a carga estará pronta para processar **eventos de manuseio** (handling events) no porto. Você pode
também mudar o destino da carga, se necessário, ou **rastrear a carga** (track cargo).

A interface de Registro de Eventos de Manuseio é destinada ao pessoal portuário que registra o que 
aconteceu com a carga. A interface é destinada principalmente a dispositivos móveis, mas
você pode usá-lo através de um navegador de mesa. A interface está acessível neste URL: http://localhost:8080/cargo-tracker/event-logger/index.xhtml. Por conveniência, você
poderia usar um emulador móvel em vez de um dispositivo móvel real. Em geral, a carga
passa por esses eventos:

* É recebido no local de origem.
* É carregado e descarregado em viagens em seu itinerário.
* É reclamado no local de destino.
* Pode passar pela alfândega em pontos arbitrários.

Ao preencher o formulário de registro do evento, é melhor ter o itinerário 
à mão. Você pode acessar o itinerário para carga registrada através da interface administrativa. O manuseio da carga é feito através de Mensagens para escalabilidade. Ao utilizar o registrador de eventos, observe que somente os eventos de carga e descarga requerem como viagem associada.

Você também deve explorar a interface de registro de eventos em massa baseada no sistema de arquivos. 
Ela lê os arquivos sob /tmp/uploads. Os arquivos são apenas arquivos CSV. Uma amostra de CSV
está disponível em [src/test/sample/handling_events.csv](src/test/sample/handling_events.csv). A amostra já está configurada para corresponder aos demais eventos do itinerário da carga ABC123. Certifique-se de atualizar os horários na primeira coluna do arquivo CSV de amostra para coincidir também com o itinerário.

As entradas processadas com sucesso são arquivadas em /tmp/archive. Quaisquer registros com falhas são 
arquivados sob /tmp/falha.

Não se preocupe em cometer erros. A aplicação pretende ser justa 
tolerante a erros. Se você se deparar com problemas, você deve [relatá-los](https://github.com/eclipse-ee4j/cargotracker/issues).

Você pode simplesmente remover ./cargo-tracker-data do sistema de arquivos para reiniciar novamente. Este diretório normalmente estará abaixo de $$$ sua instalação paga/glassfish/domínios/domínios1/config.

Você também pode usar os scripts da soapUI incluídos no código fonte para explorar o 
interfaces REST, bem como os numerosos testes unitários que cobrem a base de código 
em geral. Alguns dos testes utilizam Arquillian.

Traduzido com a versão gratuita do tradutor - www.DeepL.com/Translator




--

The Handling Event Logging interface is intended for port personnel registering what 
happened to cargo. The interface is primarily intended for mobile devices, but
you can use it via a desktop browser. The interface is accessible at this URL: http://localhost:8080/cargo-tracker/event-logger/index.xhtml. For convenience, you
could use a mobile emulator instead of an actual mobile device. Generally speaking cargo
goes through these events:

* It's received at the origin location.
* It's loaded and unloaded onto voyages on it's itinerary.
* It's claimed at it's destination location.
* It may go through customs at arbitrary points.

While filling out the event registration form, it's best to have the itinerary 
handy. You can access the itinerary for registered cargo via the admin interface. The cargo handling is done via Messaging for scalability. While using the event logger, note that only the load and unload events require as associated voyage.

You should also explore the file system based bulk event registration interface. 
It reads files under /tmp/uploads. The files are just CSV files. A sample CSV
file is available under [src/test/sample/handling_events.csv](src/test/sample/handling_events.csv). The sample is already set up to match the remaining itinerary events for cargo ABC123. Just make sure to update the times in the first column of the sample CSV file to match the itinerary as well.

Sucessfully processed entries are archived under /tmp/archive. Any failed records are 
archived under /tmp/failed.

Don't worry about making mistakes. The application is intended to be fairly 
error tolerant. If you do come across issues, you should [report them](https://github.com/eclipse-ee4j/cargotracker/issues).

You can simply remove ./cargo-tracker-data from the file system to restart fresh. This directory will typically be under $your-payara-installation/glassfish/domains/domain1/config.

You can also use the soapUI scripts included in the source code to explore the 
REST interfaces as well as the numerous unit tests covering the code base 
generally. Some of the tests use Arquillian.

## Exploring the Code

As mentioned earlier, the real point of the application is demonstrating how to 
create well architected, effective Jakarta EE applications. To that end, once you 
have gotten some familiarity with the application functionality the next thing 
to do is to dig right into the code.

DDD is a key aspect of the architecture, so it's important to get at least a 
working understanding of DDD. As the name implies, Domain-Driven Design is an 
approach to software design and development that focuses on the core domain and 
domain logic.

For the most part, it's fine if you are new to Jakarta EE. As long as you have a
basic understanding of server-side applications, the code should be good enough to get started. For learning Jakarta EE further,
we have recommended a few links in the resources section of the project site. Of 
course, the ideal user of the project is someone who has a basic working 
understanding both Jakarta EE and DDD. Though it's not our goal to become a kitchen 
sink example for demonstrating the vast amount of APIs and features in Jakarta EE,
we do use a very representative set. You'll find that you'll learn a fair amount
by simply digging into the code to see how things are implemented.

## Cloud Demo
Cargo Tracker is deployed to Kubernetes on the cloud using GitHub Actions workflows. You can find the demo deployment on the Scaleforce cloud (https://cargo-tracker.j.scaleforce.net). This project is very thankful to our sponsors [Jelastic](https://jelastic.com) and [Scaleforce](https://www.scaleforce.net) for hosting the demo! The deployment and all data is refreshed nightly. On the cloud Cargo Tracker uses PostgreSQL as the database. The [GitHub Container Registry](https://ghcr.io/eclipse-ee4j/cargo-tracker) is used to publish Docker images.

![Cargo Tracker sponsors](sponsors.png)

## Java EE 7
A Java EE 7, Java SE 8, Payara 4.1 version of Cargo Tracker is available under the ['javaee7' branch](https://github.com/eclipse-ee4j/cargotracker/tree/javaee7).

## Contributing
This project complies with the [Google Java Style Guide](https://google.github.io/styleguide/javaguide.html). You can use the [google-java-format](https://github.com/google/google-java-format) tool to help you comply with the Google Java Style Guide. You can use the tool with most major IDEs such as Eclipse and IntelliJ.

In addition, for all XML, XHTML and HTML files we use a column/line width of 100 and we use 4 spaces for indentation. Please adjust the formatting settings of your IDE accordingly.

For further guidance on contributing including the project roadmap, please look [here](CONTRIBUTING.md).

## Known Issues
* When you load the project in the Eclipse IDE, you may get some spurious validation failure messages on the XML deployment descriptors (these are essentially bugs in Eclipse). These are harmless and the application is just fine. You can simply ignore these false validation messages or delete them by going to the Markers tab.
* You may get a log message stating that Payara SSL certificates have expired. This won't get in the way of functionality, but it will
  stop log messages from being printed to the IDE console. You can solve this issue by manually removing the expired certificates from the Payara domain, as 
  explained [here](https://github.com/payara/Payara/issues/3038).
* If you restart the application a few times, you will run into a bug causing a spurious deployment failure. While the problem can be annoying, it's harmless.
  Just re-run the application (make sure to completely un-deploy the application and shut down Payara first).
* Sometimes when the server is not shut down correctly or there is a locking/permissions issue, the H2 database that 
  the application uses get's corrupted, resulting in strange database errors. If 
  this occurs, you will need to stop the application and clean the database. You 
  can do this by simply removing the cargo-tracker-data directory from the file 
  system and restarting the application. This directory will typically be under $your-payara-installation/glassfish/domains/domain1/config.
