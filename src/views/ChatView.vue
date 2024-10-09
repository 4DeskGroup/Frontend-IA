<template>
  <div class="chat-container">
    <div class="messages">
      <div v-for="(message, index) in messages" :key="index" class="message"
        :class="{ 'my-message': message.sender === 'me' }">
        <div v-html="message.text"></div> <!-- {{ message.text }} -->
      </div>
    </div>
    <div class="input-container">
      <input type="text" v-model="novaPergunta" @keyup.enter="enviarPergunta" placeholder="Digite sua mensagem..." />
      <button @click="enviarPergunta" disabled=block>
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
import Axios from 'axios'
import { ref } from 'vue';
import Vector from '../assets/Vector.svg';

export default {
  setup() {
    const novaPergunta = ref("");
    const messages = ref([]);

    async function enviarPergunta() {
      if (novaPergunta.value.trim() !== "") {
        const valuePergunta = novaPergunta.value
        novaPergunta.value = ""
        messages.value.push({ text: valuePergunta, sender: 'me' });
        
        try {
          const response = await Axios.post("http://localhost:8000/ask", {
            question: valuePergunta
          });
          
          const resposta_formatada = response.data.answer.replace(/\n/g, '<br>');
          messages.value.push({ text: resposta_formatada, sender: 'bot' });
          
        } catch (error) {
          console.error("Erro ao enviar pergunta:", error);
        }

      }
    }

    return {
      novaPergunta,
      messages,
      enviarPergunta
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
  /* Altere para o caminho da sua imagem */
  background-size: cover;
  /* Ajusta a imagem para cobrir todo o container */
  background-position: center;
  /* Centraliza a imagem */
  background-repeat: no-repeat;
  /* Evita que a imagem se repita */
}

.messages {
  flex: 1;
  overflow-y: auto;
  padding: 10px;
  margin-bottom: 10px;
}

.message {
  background-color: #F8F1FF;
  color: #775BB4;
  padding: 10px;
  border-radius: 10px;
  margin-bottom: 1rem;
  max-width: 45%;
  word-wrap: break-word;
  /* Permite a quebra de palavras longas */
  white-space: pre-wrap;
  /* Mantém quebras de linha e espaços */
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.2); /* Sombra para mensagens */
}

.my-message {
  background-color: #F8F1FF;
  color: #838383;
  margin-left: auto;
  word-wrap: break-word;
  /* Permite a quebra de palavras longas */
  white-space: pre-wrap;
  /* Mantém quebras de linha e espaços */
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.2); /* Sombra para mensagens */
}

.input-container {
  display: flex;
  gap: 10px;
}

input {
  flex: 1;
  padding: 10px;
  border-radius: 20px;
  background-color: #FDFBFF;
  border: 1px solid #ccc;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1); /* Sombra para o input */
}

button {
  padding: 10px;
  background-color: #E4E2E6;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

button:hover {
  background-color: #E4E2E6;
}
</style>