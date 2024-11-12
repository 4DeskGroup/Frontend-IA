<template>
  <div class="chat-container">
    <div class="messages">
      <div v-for="(message, index) in messages" :key="index" class="message"
        :class="{ 'my-message': message.sender === 'me' }">
        <div v-html="formatText(message.text)"></div>
      </div>
      <div v-if="isLoading" class="loading-dots">
        <span class="dot"></span>
        <span class="dot"></span>
        <span class="dot"></span>
      </div>
    </div>
    <div class="input-container">
      <input type="text" v-model="novaPergunta" @keyup.enter="enviarPergunta" placeholder="Digite sua mensagem..." />
      <button @click="enviarPergunta" :disabled="!novaPergunta">
        <svg width="18" height="18" viewBox="0 0 18 18" fill="none" xmlns="http://www.w3.org/2000/svg">
          <path
            d="M1.61688 8.66113L1.01288 3.22413C0.839883 1.66813 2.44188 0.525129 3.85688 1.19613L15.8009 6.85413C17.3259 7.57613 17.3259 9.74613 15.8009 10.4681L3.85688 16.1271C2.44188 16.7971 0.839883 15.6551 1.01288 14.0991L1.61688 8.66113ZM1.61688 8.66113H8.61688"
            stroke="#B9B9B9" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round" />
        </svg>
      </button>
    </div>
  </div>
</template>

<script>
import Axios from 'axios';
import { onMounted } from 'vue';
import { ref } from 'vue';

export default {
  setup() {
    const novaPergunta = ref("");
    const messages = ref([]);
    const isLoading = ref(false);

    // Array com as perguntas predefinidas
    const perguntas = [
      "Quais são os melhores produtos para escritório?",
      "Me recomende 5 produtos para decoração de sala de estar.",
      "Quais são os produtos mais bem avaliados para cozinha?",
      "Me fale sobre os produtos mais populares para crianças.",
      "Quais produtos são indicados para quem tem pouco espaço em casa?",
      "Quais são os melhores eletrodomésticos para cozinha?",
      "Quais são os melhores produtos para organizar a casa?",
      "Quais são os produtos mais recomendados para quem mora em apartamento?",
      "Me dê 3 opções de sofás para sala de estar.",
      "Quais são os melhores produtos para quem tem pets em casa?",
      "Quais são os produtos com melhor custo-benefício para cama e colchão?",
      "Quais são os produtos com as melhores avaliações de durabilidade?",
      "Quais produtos têm a melhor relação custo-benefício para decoração de interiores?",
      "Quais produtos têm a melhor qualidade de acabamento?",
      "Quais são os produtos mais recomendados para pessoas com alergia?",
      "Quais são os melhores produtos de limpeza para casa?",
      "Me diga os 3 produtos com maior avaliação de resistência no mercado.",
      "Quais são os melhores modelos de mesa de jantar com bom custo-benefício?",
      "Quais produtos têm as melhores avaliações sobre desempenho e uso diário?",
      "Qual é o melhor produto para quem procura uma mesa de escritório durável?",
      "Quais são os melhores produtos com controle remoto para sala de estar?",
      "Quais sofás reclináveis têm os melhores reviews?",
      "Quais são os produtos mais recomendados para iluminação de ambientes internos?",
      "Quais são as melhores cadeiras ergonômicas para escritório?",
      "Quais são as melhores cafeteiras automáticas para casa?",
      "Me fale sobre os melhores sistemas de som para TV.",
      "Quais produtos para organização de cozinha têm mais funcionalidades?",
      "Quais são as melhores opções de ventiladores para quarto?",
      "Quais são os melhores aspiradores de pó para pets?",
      "Quais são os melhores purificadores de ar?",
      "Quais produtos estão em promoção para sala de estar?",
      "Quais são os produtos mais acessíveis em relação a colchões de casal?",
      "Me recomende produtos baratos e bons para decoração de sala.",
      "Quais são os produtos mais acessíveis para cozinha, mas de boa qualidade?",
      "Quais produtos para casa têm os melhores descontos no e-commerce?",
      "Quais são as melhores ofertas de TVs no mercado?",
      "Quais são os produtos de decoração mais baratos para um apartamento pequeno?",
      "Me fale sobre as promoções de móveis para sala.",
      "Quais são os melhores produtos de jardinagem com preço acessível?",
      "Quais são as opções de cadeiras de escritório com bom preço?",
      "Qual é melhor: sofá de couro ou sofá de tecido?",
      "Qual colchão é mais confortável: ortopédico ou de espuma?",
      "Qual é o melhor modelo de micro-ondas: 20L ou 30L?",
      "O que é melhor para iluminação de ambientes: lâmpada LED ou fluorescente?",
      "Qual produto tem melhor desempenho: aspirador de pó vertical ou robotizado?",
      "Qual é o melhor tipo de panela para cozinha: inox ou antiaderente?",
      "Qual é a melhor opção de cama: box ou cama com cabeceira?",
      "Comparando as melhores opções de geladeiras, qual tem melhor custo-benefício?",
      "Qual é o melhor tipo de sofá para quem tem filhos: retrátil ou modular?",
      "Qual é o melhor produto de limpeza para pisos frios: cerâmica ou porcelanato?"
    ];

    async function enviarPergunta() {
      if (novaPergunta.value.trim() !== "") {
        const valuePergunta = novaPergunta.value;
        novaPergunta.value = "";
        messages.value.push({ text: valuePergunta, sender: 'me' });

        isLoading.value = true; // Inicia a animação de loading

        try {
          const response = await Axios.post("http://localhost:8000/ask", {
            question: valuePergunta
          });

          const resposta_formatada = response.data.answer.replace(/\n/g, '<br>');
          messages.value.push({ text: resposta_formatada, sender: 'bot' });
        } catch (error) {
          console.error("Erro ao enviar pergunta:", error);
        } finally {
          isLoading.value = false; // Finaliza a animação de loading
        }
      }
    }

    // Função para formatar o texto com base nos asteriscos
    function formatText(text) {
      let formattedText = text.replace(/\*\*(.*?)\*\*/g, '<span class="bold-text">$1</span>'); // Quando o texto vem com ** ** ele transforma para negrito
      formattedText = formattedText.replace(/\*/g, '-');  // Quando vier apenas um * ele troca para -

      return formattedText;
    }

    async function reiniciar() {
      try {
        await Axios.put("http://localhost:8000/clear");
      } catch (error) {
        console.error("Erro ao reiniciar:", error);
      }
    }

    // Chama a função reiniciar quando a tela for carregada ou recarregada
    onMounted(() => {
      reiniciar();
    });

    return {
      novaPergunta,
      messages,
      isLoading,
      enviarPergunta,
      formatText
    };
  }
}
</script>

<style scoped>
.chat-container {
  display: flex;
  flex-direction: column;
  height: 100vh;
  width: 190vh;
  justify-content: flex-end;
  padding: 10px;
  background-image: url('../assets/scrn-fundo.png');
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
}

.messages {
  flex: 1;
  /*overflow-y: auto;*/
  overflow-y: auto;
  /* Apenas rolagem vertical */
  overflow-x: hidden;
  /* Evita rolagem lateral */
  padding: 10px;
  margin-bottom: 10px;
}

.message {
  background-color: #ffffff;
  color: #775BB4;
  padding: 10px;
  border-radius: 10px;
  margin-bottom: 1rem;
  max-width: 45%;
  word-wrap: break-word;
  white-space: pre-wrap;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.2);
}

.my-message {
  background-color: #ffffff;
  color: #838383;
  margin-left: auto;
  word-wrap: break-word;
  white-space: pre-wrap;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.2);
}

.input-container {
  position: relative;
  /* Define o contêiner como relativo */
  display: flex;
  gap: 10px;
  /* Mantenha o gap se necessário, mas não afetará a posição do botão */
}

input {
  flex: 1;
  padding: 10px;
  border-radius: 20px;
  background-color: #FDFBFF;
  border: 1px solid #ccc;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
  padding-right: 40px;
  /* Adiciona espaço à direita para o botão */
}

input:focus {
  outline: none;
  /* Remove a borda de foco padrão */
}

button {
  position: absolute;
  /* Faz o botão ficar sobre o input */
  right: 10px;
  /* Distância da borda direita */
  top: 50%;
  /* Centraliza verticalmente */
  transform: translateY(-50%);
  /* Ajusta para centralização perfeita */
  padding: 10px;
  background-color: transparent;
  color: white;
  border: none;
  border-radius: 50%;
  /* Faz o botão circular */
  cursor: pointer;
}

button:hover {
  background-color: #E4E2E6;
}

/* Estilos para os pontos de loading */
.loading-dots {
  display: flex;
  justify-content: start;
  margin: 1rem 0;
}

.dot {
  width: 8px;
  height: 8px;
  margin: 0 4px;
  background-color: #775BB4;
  /* Cinza escuro */
  border-radius: 50%;
  animation: bounce 0.6s infinite alternate;
}

.dot:nth-child(2) {
  animation-delay: 0.2s;
}

.dot:nth-child(3) {
  animation-delay: 0.4s;
}

@keyframes bounce {
  from {
    transform: translateY(0);
  }

  to {
    transform: translateY(-10px);
  }
}
</style>
