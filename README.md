# c-digo-jardineiro-agrinho
let jardineiro;
let plantas = [];
let temperatura = 10;
let totalArvores = 0; // Variável global para contar as árvores

function setup() {
  createCanvas(600, 400);
  jardineiro = new Jardineiro(width / 2, height - 50);
}

function draw() {
  // A cor de fundo muda de laranja para verde claro dependendo do número de árvores
  let corFundo = lerpColor(color(217, 112, 26), color(219, 239, 208),
    map(totalArvores, 0, 100, 0, 1)); // Mapeia 0-100 árvores para 0-1 na interpolação de cor

  background(corFundo);
  mostrarInformacoes();
  temperatura += 0.05; // A temperatura aumenta gradualmente (diminuído para 0.05 para ser mais lento)
  jardineiro.atualizar();
  jardineiro.mostrar();
  verificarFimDeJogo();

  // Mapeia sobre o array de plantas para mostrar cada árvore
  plantas.map((arvore) => arvore.mostrar());
}

function mostrarInformacoes() {
  textSize(16);
  fill(0);
  text("Vamos plantar arvores para reduzir a temperatura!", 10, 30); // Pequena correção na frase
  textSize(14);
  fill('white');
  text("Temperatura: " + temperatura.toFixed(2) + "°C", 10, 390); // Adicionado °C
  text("Árvores plantadas: " + totalArvores, 460, 390);
  text("Para movimentar o personagem use as setas do teclado.", 10, 60);
  text("Para plantar árvores use P ou espaço.", 10, 80);
}

function verificarFimDeJogo() {
  // Condição de vitória: se o total de árvores (multiplicado por um fator para balancear)
  // for maior que a temperatura atual. Ajustei o fator para 5, você pode mudar.
  if (totalArvores * 2 > temperatura) {
    mostrarMensagemDeVitoria();
  } else if (temperatura > 50) { // Condição de derrota: se a temperatura ultrapassar 50
    mostrarMensagemDeDerrota();
  }
}

function mostrarMensagemDeVitoria() {
  textSize(20);
  fill(0, 0, 0);
  text("🥳 Você venceu! Você plantou muitas árvores!", 100, 200); // Adicionado emoji de festa
  noLoop(); // Para o loop draw(), congelando a tela
}

function mostrarMensagemDeDerrota() {
  textSize(20);
  fill(0, 0, 0);
  text("😒 Você perdeu! A temperatura está muito alta.", 100, 200);
  noLoop(); // Para o loop draw(), congelando a tela
}

class Jardineiro {
  constructor(x, y) {
    this.x = x;
    this.y = y;
    this.emoji = '🧑‍🌾';
    this.velocidade = 3; // Velocidade de movimento do jardineiro
  }

  atualizar() {
    // Movimento do jardineiro com as setas do teclado
    if (keyIsDown(LEFT_ARROW)) {
      this.x -= this.velocidade;
    }
    if (keyIsDown(RIGHT_ARROW)) {
      this.x += this.velocidade;
    }
    if (keyIsDown(UP_ARROW)) {
      this.y -= this.velocidade;
    }
    if (keyIsDown(DOWN_ARROW)) {
      this.y += this.velocidade;
    }

    // Limitar o jardineiro dentro da área do canvas
    // Considera um tamanho aproximado do emoji para não sair pela borda direita/inferior
    this.x = constrain(this.x, 0, width - 32);
    this.y = constrain(this.y, 0, height - 32);
  }

  mostrar() {
    textSize(32); // Tamanho do emoji do jardineiro
    text(this.emoji, this.x, this.y);
  }
}

// ---
// **Função Corrigida:** Esta é a função que será chamada quando uma tecla for pressionada.
function keyPressed() {
  // Verifica se a tecla pressionada é 'p' (maiúscula ou minúscula) ou a barra de espaço
  if (key === 'p' || key === 'P' || keyCode === 32) { // 32 é o keyCode para a barra de espaço
    // Cria uma nova instância da classe Arvore na posição do jardineiro
    // Adicionei um pequeno ajuste (+10 no Y) para a árvore aparecer um pouco abaixo do jardineiro
    let arvore = new Arvore(jardineiro.x, jardineiro.y + 10);
    plantas.push(arvore); // Adiciona a nova árvore ao array de plantas

    totalArvores++; // **Correção:** Incrementa a variável 'totalArvores'
    temperatura -= 3; // Reduz a temperatura ao plantar uma árvore

    // Garante que a temperatura não fique abaixo de zero
    if (temperatura < 0) {
      temperatura = 0;
    }
  }
}
// ---

class Arvore {
  constructor(x, y) {
    this.x = x;
    this.y = y;
    this.emoji = '🌳'; // O emoji da árvore
  }

  mostrar() {
    textSize(32); // Tamanho do emoji da árvore
    text(this.emoji, this.x, this.y);
  }
}
