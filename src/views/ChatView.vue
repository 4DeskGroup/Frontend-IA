<template>
  <div class="chat-container">
    <div class="messages">
      <div
        v-for="(message, index) in messages"
        :key="index"
        class="message"
        :class="{ 'my-message': message.sender === 'me' }"
      >
        {{ message.text }}
      </div>
    </div>
    <div class="input-container">
      <input
        type="text"
        v-model="novaPergunta"
        @keyup.enter="enviarPergunta"
        placeholder="Digite sua mensagem..."
      />
      <button @click="enviarPergunta">Enviar</button>
    </div>
  </div>
</template>

<script>
import Axios from 'axios'
import { ref } from 'vue';

export default {
  setup() {
    const novaPergunta = ref("");
    const messages = ref([]);

    async function enviarPergunta() {
      if (novaPergunta.value.trim() !== "") {
        messages.value.push({ text: novaPergunta.value, sender: 'me' });
        
        try {
          const response = await Axios.post("http://localhost:8000/ask", {
            question: novaPergunta.value
          });
          console.log(response)
          // Adiciona a resposta da API
          messages.value.push({ text: response.data.answer, sender: 'bot' });
          
        } catch (error) {
          console.error("Erro ao enviar pergunta:", error);
        }

        // Limpa o campo de input
        novaPergunta.value = "";
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
    /* position: absolute; */
    justify-content: flex-end;
    padding: 10px;
  }

  .messages {
    flex: 1;
    overflow-y: auto;
    padding: 10px;
    border: 1px solid #ccc;
    margin-bottom: 10px;
  }

  .message {
    background-color: #f0f5f4;
    color: black;
    padding: 10px;
    border-radius: 10px;
    margin-bottom: 5px;
    max-width: 70%;
  }

  .my-message {
    background-color: #007bff;
    color: white;
    margin-left: auto;
  }

  .input-container {
    display: flex;
    gap: 10px;
  }

  input {
    flex: 1;
    padding: 10px;
    border-radius: 5px;
    border: 1px solid #ccc;
  }

  button {
    padding: 10px;
    background-color: #007bff;
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
  }

  button:hover {
    background-color: #0056b3;
  }
</style>