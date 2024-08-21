let player; // Variável para o jogador
let obstacles = []; // Array para armazenar os obstáculos
let score = 0; // Variável para a pontuação

function setup() {
  createCanvas(400, 600); // Cria um canvas de 400x600 pixels
  player = new Player(); // Inicializa o jogador
  
  // Define a taxa de criação de obstáculos (quanto menor, mais rápido os obstáculos aparecem)
  frameRate(30);
}

function draw() {
  background(0); // Preenche o fundo com preto
  
  player.show(); // Mostra o jogador
  player.update(); // Atualiza a posição do jogador
  
  // Cria novos obstáculos aleatoriamente
  if (random(1) < 0.03) { // Probabilidade de criar um novo obstáculo
    obstacles.push(new Obstacle()); // Adiciona um novo obstáculo ao array
  }
  
  // Percorre todos os obstáculos e os mostra, além de atualizar suas posições
  for (let i = obstacles.length - 1; i >= 0; i--) {
    obstacles[i].show();
    obstacles[i].update();
    
    // Verifica se o jogador colidiu com algum obstáculo
    if (obstacles[i].hits(player)) {
      console.log("Game Over!");
      noLoop(); // Para o loop principal do jogo
    }
    
    // Remove obstáculos que saíram da tela para economizar memória
    if (obstacles[i].offscreen()) {
      obstacles.splice(i, 1); // Remove o obstáculo do array
      score++; // Incrementa a pontuação
    }
  }
  
  // Mostra a pontuação na tela
  textSize(32);
  fill(255);
  text("Score: " + score, 20, 50);
}

// Classe para o jogador
class Player {
  constructor() {
    this.width = 50;
    this.height = 10;
    this.x = width / 2 - this.width / 2;
    this.y = height - 40;
    this.xSpeed = 0;
    this.speed = 6; // Velocidade do jogador
  }
  
  // Atualiza a posição do jogador com base na velocidade
  update() {
    this.x += this.xSpeed;
    this.x = constrain(this.x, 0, width - this.width); // Limita o jogador dentro da tela
  }
  
  // Mostra o jogador na tela
  show() {
    fill(255);
    rect(this.x, this.y, this.width, this.height);
  }
}

// Classe para os obstáculos
class Obstacle {
  constructor() {
    this.width = random(10, 50);
    this.height = 10;
    this.x = random(width - this.width);
    this.y = 0 - this.height;
    this.ySpeed = random(3, 8); // Velocidade de queda do obstáculo
  }
  
  // Atualiza a posição do obstáculo
  update() {
    this.y += this.ySpeed;
  }
  
  // Mostra o obstáculo na tela
  show() {
    fill(255, 0, 0);
    rect(this.x, this.y, this.width, this.height);
  }
  
  // Verifica se o obstáculo colidiu com o jogador
  hits(player) {
    return (this.y + this.height >= player.y && this.y <= player.y + player.height &&
            this.x + this.width >= player.x && this.x <= player.x + player.width);
  }
  
  // Verifica se o obstáculo saiu da tela
  offscreen() {
    return (this.y > height);
  }
}

// Função para controlar o jogador com as teclas
function keyPressed() {
  if (keyCode === LEFT_ARROW) {
    player.xSpeed = -player.speed; // Move para a esquerda
  } else if (keyCode === RIGHT_ARROW) {
    player.xSpeed = player.speed; // Move para a direita
  }
}

// Função para parar o movimento do jogador quando a tecla é solta
function keyReleased() {
  if (keyCode === LEFT_ARROW || keyCode === RIGHT_ARROW) {
    player.xSpeed = 0; // Para o jogador
  }
}
