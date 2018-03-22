![logo do node girls](https://i.imgur.com/4QATtIp.png)

# Arduíno com Johnny Five, Firmata e Node.js

Vamos explicar direitinho como funciona a configuração e alguns comandinhos básicos para você ficar feliz dando os primeiros passos com arduíno :)

## Instalação

E aí, mãos na massa?

### Pré-requisitos

Você precisa ter o Node.js instalado em sua máquina e o Git também :) Qualquer um dos dois está disponível facilmente na internet. Aqui (https://nodejs.org/en) seria o Node e no meu caso que uso Windows, está aqui o Git Bash: https://gitforwindows.org! Se você tem Linux, corre aqui: https://git-scm.com/download/linux :D

Ah, também precisa ter a IDE do arduíno :) Está aqui: https://www.arduino.cc/en/Main/Software

Neste tutorial vamos usar como base o Arduino Uno!

### Começou!

* Primeiro de tudo, depois de feito o download do software do arduíno de acordo com os requisitos da sua máquina, plugue o arduíno nela e veja se os drivers instalam com sucesso.

Se der problema para instalar e o seu PC não reconhecer o arduíno, você vai fazer o seguinte. Vai no Gerenciador de Dispositivos da sua máquina, procurar em Outros Dispositivos o seu arduíno perdido como "Dispositivo Desconhecido", clicar com o botão direito em Instalar ou Atualizar Driver. Selecione a opção de procurar dentro do computador o driver e selecione a pasta que está dentro da instalação do arduíno que você acabou de baixar, que é algo parecido com isso: ```C:\Users\seu usuário\Downloads\arduino-1.8.5-windows\arduino-1.8.5\drivers```. Se seu PC for Linux, também é caso de instalar os drivers. Pergunte-nos como em http://nodegirlscode.org/contact ou abra um issue.

* Espere e provavelmente ele vai conseguir instalar tudo pra ti. Beleza? :)

* Agora vamos executar o ```arduino.exe``` que fica dentro da pasta que baixamos do site do arduíno. Vamos ver se ele está funcionando, beleza?

* Abriu uma janelinha azul bem bonitinha. Vamos ir em Ferramentas > Placa e selecionar o arduíno que estamos trabalhando. O meu é o Uno :)

* Vamos agora ver se a porta está devidamente configurada. Geralmente essas coisas já estão selecionadas, a gente só vê se tá ok. Ferramenrtas > Porta e verifique qual porta COM ficou seu arduíno. Se não está em nenhuma, reveja a instalação do driver ou nos mande uma dúvida no http://nodegirlscode.org/contact, pois alguma coisa se perdeu.

* Depois, colocamos um led com uma perninha no GND e outro no número 13 da placa. Aí, vamos em Arquivo > Exemplos > Basics > Blink.

* Vai carregar um código parecido com esse:

```
/*
  Blink

  Turns an LED on for one second, then off for one second, repeatedly.

  Most Arduinos have an on-board LED you can control. On the UNO, MEGA and ZERO
  it is attached to digital pin 13, on MKR1000 on pin 6. LED_BUILTIN is set to
  the correct LED pin independent of which board is used.
  If you want to know what pin the on-board LED is connected to on your Arduino
  model, check the Technical Specs of your board at:
  https://www.arduino.cc/en/Main/Products

  modified 8 May 2014
  by Scott Fitzgerald
  modified 2 Sep 2016
  by Arturo Guadalupi
  modified 8 Sep 2016
  by Colby Newman

  This example code is in the public domain.

  http://www.arduino.cc/en/Tutorial/Blink
*/

// the setup function runs once when you press reset or power the board
void setup() {
  // initialize digital pin LED_BUILTIN as an output.
  pinMode(LED_BUILTIN, OUTPUT);
}

// the loop function runs over and over again forever
void loop() {
  digitalWrite(LED_BUILTIN, HIGH);   // turn the LED on (HIGH is the voltage level)
  delay(1000);                       // wait for a second
  digitalWrite(LED_BUILTIN, LOW);    // turn the LED off by making the voltage LOW
  delay(1000);                       // wait for a second
}
```
Nesse código a led tem que apagar e acender de um em um segundo. Vamos clicar em Carregar, que é um ícone com uma seta apontada para a direita.

* É para sua led estar piscando depois de carregado o código. Se não piscou, pode ser que sua led esteja com a perninha trocada. Coloque a perna que está no GND no 13 e vice-versa. Temos sempre que nos atentar para colocar a perninha do "terra" no lugar certo, que é no GND da placa :)

Pronto. Seu arduíno funciona! Vamos codar em Javascript?

### Johnny-Five e Firmata :p

* Vamos criar uma pasta para o nosso projeto e dentro dela dar um ```npm init --yes``` para criar um package.json. É só abrir o Git Bash com o botão direito dentro da pasta e colar esse script com shift+insert e dar enter.

* Agora, vamos instalar o Johnny Five! Ele que faz a gente conseguir manipular via Javascript o nosso arduíno :)

```npm install johnny-five```

* Com o JF instalado, você pode agora criar dentro do seu projeto um arquivo ```index.js``` que contém o seguinte código:

```
var five = require("johnny-five");
var board = new five.Board();

// The board's pins will not be accessible until
// the board has reported that it is ready
board.on("ready", function() {
  console.log("baladinha de ratoooooooooooOOOOO");

  var led = new five.Led(13);
  led.blink(50);
});
```

* Uma observação importante é que se você trabalha com Linux, terá que liberar a porta COM utilizando o código ```sudo chmod a+rw /dev/ttyACM0```

* Mas ainda assim, se tentarmos rodar com ```node index.js``` nosso código no Git Bash vai dar erro, porque não temos o Firmata. Vamos instalar então :)

```npm install firmata-party```

* Depois de instalado, vamos criar no package.json um script para resetar nosso arduíno antes que executar os códigos nele, para que funcione.

```
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "reset": "firmata-party uno --debug"
  },
  ```
  
* Vamos rodar então para resetar:

```npm run reset```

* E agora para rodar o código:

```node index.js```

* Se eu quiser interagir em prompt de comando lá no Git Bash com o código, eu insiro no index.js embaixo de ```led.blink(50);```

```  this.repl.inject({led: led}); ```

Isso vai fazer com que consigamos configurar o led do nosso arduíno no próprio Git Bash. Se usarmos comandos como led.on(), o led só acende. Se usarmos led.stop(), o led apaga. Se usarmos led.strobe(), ele volta a piscar como definimos no código, ou ainda podemos definir dentro da função com led.strobe(2000), sendo acendido e apagado a cada 2 segundos.

Muito bacana, né? :)

