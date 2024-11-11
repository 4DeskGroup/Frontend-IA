<template>
  <div class="chat-container">
    <div class="messages">
      <div v-for="(message, index) in messages" :key="index" class="message"
        :class="{ 'my-message': message.sender === 'me' }">
        <div v-html="message.text"></div>
      </div>
      <div v-if="isLoading" class="loading-dots">
        <span class="dot"></span>
        <span class="dot"></span>
        <span class="dot"></span>
      </div>
    </div>

    <div class="input-container">
      <button class="botao-sugestao" @click="toggleBaloes">
        <i class="fa-solid fa-lightbulb"></i>
      </button>
      <input type="text" v-model="novaPergunta" @keyup.enter="enviarPergunta" placeholder="Digite sua mensagem..." />
      <button @click="enviarPergunta" :disabled="!novaPergunta">
        <svg width="18" height="18" viewBox="0 0 18 18" fill="none" xmlns="http://www.w3.org/2000/svg">
          <path
            d="M1.61688 8.66113L1.01288 3.22413C0.839883 1.66813 2.44188 0.525129 3.85688 1.19613L15.8009 6.85413C17.3259 7.57613 17.3259 9.74613 15.8009 10.4681L3.85688 16.1271C2.44188 16.7971 0.839883 15.6551 1.01288 14.0991L1.61688 8.66113ZM1.61688 8.66113H8.61688"
            stroke="#B9B9B9" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round" />
        </svg>
      </button>
    </div>

    <div v-if="showBaloes" class="baloes-container">
      <div v-for="(baloon, index) in baloes" :key="index" class="baloon">
        {{ baloon }}
      </div>
    </div>
  </div>
</template>

<script>
import Axios from 'axios';
import '@fortawesome/fontawesome-free/css/all.min.css';
import { onMounted } from 'vue';
import { ref } from 'vue';

export default {
  setup() {
    const novaPergunta = ref("");
    const messages = ref([]);
    const isLoading = ref(false);
    const showBaloes = ref(false);

    const baloes = ref([
      "Comparação de produtos",
      "Sugestão de produtos",
      "Tendência de segmentos",
      "Comparação de produtos"
    ]);

    function toggleBaloes() {
      showBaloes.value = !showBaloes.value;
    }

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

    async function reiniciar() {
      try {
        await Axios.put("http://localhost:8000/clear");
      } catch (error) {
        console.error("Erro ao reiniciar:", error);
      }
    }

    onMounted(() => {
      reiniciar();
    });

    return {
      novaPergunta,
      messages,
      isLoading,
      enviarPergunta,
      showBaloes,
      toggleBaloes,
      baloes
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
  background-image: url('../assets/src-fundo-novo.png');
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
}

.messages {
  flex: 1;
  /*overflow-y: auto;*/
  overflow-y: auto;
  overflow-x: hidden;
  padding: 10px;
  margin-bottom: 10px;
}

.message {
  background-color: #FFFFFF;
  color: #380986;
  padding: 10px;
  border-radius: 10px;
  margin-bottom: 1rem;
  max-width: 45%;
  word-wrap: break-word;
  white-space: pre-wrap;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.2);
}

.my-message {
  background-color: #F8F1FF;
  color: #100B4A;
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
  right: 10px;
  top: 50%;
  transform: translateY(-50%);
  padding: 10px;
  background-color: transparent;
  color: white;
  border: none;
  border-radius: 50%;
  cursor: pointer;
}

button:hover {
  background-color: #E4E2E6;
}

.botao-sugestao {
  position: absolute;
  right: 11px;
  top: -75%;
  transform: translateY(-50%);
  padding: 10px;
  background-color: white;
  color: white;
  border: none;
  border-radius: 50%;
  cursor: pointer;
}

.botao-sugestao i {
  color: #380986; /* Cor roxa para o ícone */
  font-size: 24px;
}

.botao-sugestao:hover {
  background-color: #F0F0F0;
}

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

.baloes-container {
  display: flex;
  gap: 10px;
  justify-content: center;
  margin-bottom: 10px;
}

.baloon {
  padding: 8px 16px;
  background-color: #FFFFFF;
  color: #380986;
  border-radius: 20px;
  box-shadow: 0px 2px 4px rgba(0, 0, 0, 0.1);
  font-size: 0.9rem;
  cursor: pointer;
}
</style>