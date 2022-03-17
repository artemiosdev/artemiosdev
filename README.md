### Hello everyone👋🥳 
#### Glad to see you here! 🤩   

My name is Artem and I'm a junior iOS developer🍏. I'm from Russia, living in Krasnodar (The Black Sea).   

<!-- In my Instagram, I write about my journey as an early-career professional.  Follow me and join my dev coding journey ♥️✌️ -->

<p align="left">
  <a href="mailto:flyboroda@gmail.com"><img src="https://img.icons8.com/color/96/000000/gmail--v1.png"/ alt="Email"/></a>
 <!--  <a href="https://twitter.com/artobor"><img src="https://img.icons8.com/color/96/000000/twitter-squared.png" alt="twitter"/></a> -->
 <!--  <a href="https://www.instagram.com/artem_iosdev/"><img src="https://img.icons8.com/color/96/000000/instagram-new.png" alt="instagram"/></a> -->
  <a href="https://vk.com/artobor"><img src="https://img.icons8.com/fluency/96/000000/vk-circled.png"/ alt="VK"/></a>
  <a href="https://t.me/artobor"><img src="https://img.icons8.com/color/96/000000/telegram-app--v5.png"/ alt="Telegram"/></a>
</p>

<!-- [![Twitter](https://github-readme-twitter.gazf.vercel.app/api?id=artobor&layout=wide)](https://twitter.com/artobor) -->


I'm currently working on 🔭 building Mobile Apps on iOS using Swift (UIKit + SwiftUI)   

I like programming, design, photography. I love to explore and learn about new things  

💬 Ask me about anything [here](https://github.com/artemiosdev/artemiosdev/issues)

<p align="left">
  <img src="https://github-readme-stats.vercel.app/api?username=artemiosdev&show_icons=true&theme=radical" alt="Artem Androsenko artemiosdev Github Stats"></img>
</p>

![](https://komarev.com/ghpvc/?username=artemiosdev&style=flat-square&label=Views)
![](https://badges.pufler.dev/visits/artemiosdev/artemiosdev?color=black&logo=github&style=flat-square)

<div>
<!DOCTYPE html>
<html lang="ru" >
<head>
  <meta charset="UTF-8">
  <title>Текстовый дождь из тучки</title>
  <!-- подключаем стили для красивого оформления символов -->
  <link rel='stylesheet' href='https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.8.2/css/all.min.css'>
  <style type="text/css">
  	/* сделаем так, чтобы мы могли задавать размеры всех элементов в разных единицах одновременно, например, высоту в пикселях, а ширину в процентах */
  	*{
  		box-sizing: border-box;
  	}
  	/* общие настройки для страницы */
  	body{
  		/* убираем отступы */
  		margin: 0;
  		padding: 0;
  		/* занимаем всю ширину и высоту окна браузера */
  		width: 100%;
  		height: 100vh;
  		/* не рисуем то, что вылетело за границы */
  		overflow: hidden;
  		/* цвет фона */
  		background: #2d3436;
  		/* располагаем элементы страницы по центру*/
  		display: flex;
  		justify-content: center;
  		align-items: center;
  	}

  	/* настройки области, где будет нарисована тучка */
  	.cloud-svg{
  		/* высота и ширина области */
  		width: 18rem;
  		height: 18rem;
  		position: fixed;
  		/* центрируем по горизонтали */
  		top: 120px;
  		left: 50%;
  		transform: translate(-50%, -50%);
  		/* поднимаем тучку выше остальных элементов на странице, чтобы она прятала символы, которые ещё не появились*/
  		z-index: 10;
  	}

  	/* настройки для отрисовки самой тучки */
  	.cloud{
  		/* настройки фона и контура тучки */
  		fill: #2ecc71;
  		stroke: #27ae60;
  		stroke-width: 2px;
  		stroke-linecap: round;
  		stroke-miterlimit: 10;
  	}

  	/* настройка области, где будут падающие буквы */
  	canvas{
  		position: fixed;
  		/* отступ сверху */
  		top: 350px;
  		/* центрируем по горизонтали*/
  		left: 50%;
  		transform: translate(-50%, -50%);
  	}

  </style>

</head>
<body>

	<!-- рисуем тучку -->
	<div id="wrapper">
		<svg class="cloud-svg" x="0px" y="0px" viewBox="0 0 60 60" style="enable-background:new 0 0 60 60;">
			<path class="cloud" d="M50.003,27 c-0.115-8.699-7.193-16-15.919-16c-5.559,0-10.779,3.005-13.661,7.336C19.157,17.493,17.636,17,16,17c-4.418,0-8,3.582-8,8 c0,0.153,0.014,0.302,0.023,0.454C8.013,25.636,8,25.82,8,26c-3.988,1.912-7,6.457-7,11.155C1,43.67,6.33,49,12.845,49h24.507 c0.138,0,0.272-0.016,0.408-0.021C37.897,48.984,38.031,49,38.169,49h9.803C54.037,49,59,44.037,59,37.972 C59,32.601,55.106,27.961,50.003,27z"/>
			<!-- дополнительные штрихи на тучке для объёма -->
			<path class="cloud" d="M50.003,27 c0,0-2.535-0.375-5.003,0"/>
			<path class="cloud" d="M8,25c0-4.418,3.582-8,8-8 s8,3.582,8,8"/>
		</svg>
	</div>

	<!-- подключаем p5 — библиотеку для рисования в JavaScrpt -->
  <script src='https://cdnjs.cloudflare.com/ajax/libs/p5.js/0.8.0/p5.min.js'></script>

	<script type="text/javascript">
		// размеры области, где будет падать текст
		var height = 420, width = 250;
		// на старте массив символов пуст
		var symbols = [];
		// размер символа
		var symbolSize = 10;
		// переменная для таймера
		var timer = 0;
		// на старте массив с потоками из символов тоже пуст
		var streams = [];

		// класс, на основе которого будут сделаны все символы, падающие из тучки
		class Symbol{
			// конструктор, который вызывается при создании каждого объекта на основе этого класса
			constructor(x, y, speed){
				// координаты и скорость нового символа
				this.x = x;
				this.y = y;
				this.speed = speed;
				// время переключения на новый символ
				this.switchInterval = round(random(5, 20));
				this.value;
				// метод, который выбирает символ из случайного диапазона 
				this.setRandomSymbol = function(){
					// если пришло время обновить символ —
					if(timer % this.switchInterval == 0){
						// берём новый из случайного диапазона
						this.value = String.fromCharCode(
							// если поменять стартовый диапазон — поменяется и язык символов
							0x10A0 + round(random(0, 96))
						);
					}
				}
			}
			// метод, который отвечает за падение символов
			rain(){
				// если ещё не достигли нижней границы — символ смещается вниз с установленной скоростью
				this.y = (this.y >= height) ? 0: this.y += this.speed;
			}
		}

		// класс, на основе которого будут сделаны все потоки, состоящие из нескольких символов
		class Stream{
			// конструктор, который вызывается при создании каждого объекта на основе этого класса	
			constructor(){
				// на старте в каждом потоке ничего нет
				this.stream = [];
				// в каждом потоке будет от 2 до 5 символов
				this.streamLength = round(random(2,5));
				// выбираем скорость потока случайным образом
				this.speed = random(3, 8);
			}
			// метод, который создаёт последовательность символов в потоке
			generateSymbols(x,y){
				// пока есть свободные места в массиве с потоком
				for(let i = 0; i <= this.streamLength; i++){
					// создаём новый объект-символ на основе класса для символов
					var symbol = new Symbol(x, y, this.speed);
					// формируем его значение случайным образом встроенным методом
					symbol.setRandomSymbol();
					// отправляем этот символ в массив с потоком
					this.stream.push(symbol);
					// следующий символ в потоке добавится под уже существующими
					y -= symbolSize;
				}
			}
			// метод, который отрисовывает поток символов на экране
			render(){
				// обрабатываем каждый символ в потоке
				this.stream.forEach(symbol => {
					// устанавливаем цвет символов
					fill('#00b894');
					// пишем на экране нужный символ по заданным координатам
					text(symbol.value, symbol.x, symbol.y)
					// обновляем символы, чтобы они менялись, как в Матрице
					symbol.setRandomSymbol();
					// смещаем их вниз, чтобы создать эффект дождя
					symbol.rain();
				});
			}
		}

		// подготавливаем всё к запуску — то, что написано здесь выполнится автоматически сразу после загрузки
		function setup(){
			// задаём размеры холста и рисуем его
			var height = 420, width = 220;
			var c = createCanvas(width, height);
			// находим блок с облачком по имени
			c.parent('wrapper')
			var x = 0;
			// пока у нас есть место по ширине для потоков из тучки
			for(let i = 0; i <= width/symbolSize; i++){
				// все потоки формируем внутри самой тучки, невидимо для зрителя
				var y = random(-300, 0);
				// создаём новый поток на основе класса
				var stream = new Stream();
				// заполняем его символами
				stream.generateSymbols(x, y);
				// и отправляем в массив с потоками
				streams.push(stream);
				// сдвигаем координаты вправо для следующего потока
				x += symbolSize;
			}
			// устанавливаем размер символов
			textSize(symbolSize)
		}
		// пока мы не закроем страницу, постоянно будет выполняться функция draw() 
		function draw(){
			// цвет фона
			background('#2d343680');
			// берём каждый поток…
			streams.forEach(stream => {
				// … и отрисовываем его
				stream.render();
			})
			// при каждой отрисовке увеличиваем значение таймера (оно нам нужно для смены символов через определённое время)
			timer++;
		}

	</script>

</body>
</html>
</div>
