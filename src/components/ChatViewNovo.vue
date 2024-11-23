<template>
    <div class="body">
        <div class="padding left-padding"></div>
        <div class="content">
            <div class="header">
                <span class="logo">🌸 I.R.I.S</span>
            </div>
            <div class="imagem">
                <!-- Conteúdo de imagem principal -->
            </div>
            <div class="messages">
                <!-- Contêiner de mensagens sobrepostas -->
                <div class="message">MESSAGE</div>
                <div class="my-message">MY MESSAGE</div>
            </div>
            <div class="suggestions">
                <!-- Sugestões na parte inferior -->
                <button class="suggestion">Suggestion 1</button>
                <button class="suggestion">Suggestion 2</button>
                <button class="suggestion">Suggestion 3</button>
            </div>
            <div class="input">
                <input type="text" placeholder="Digite sua mensagem aqui..." />
            </div>
        </div>
        <div class="padding right-padding"></div>
    </div>
</template>

<script>
export default {
    name: 'ChatLayout',
};
</script>

<style scoped>
/* Estilos gerais */

.body {
    display: flex;
    flex-direction: column;
    height: 100vh;
    width: 100vw;
    background-color: #f0f0f0;
}

.padding {
    flex-shrink: 0;
    width: 5%;
    background-color: #c8e6c9;
}

.left-padding {
    border-right: 2px solid red;
}

.right-padding {
    border-left: 2px solid red;
}

.content {
    flex: 1;
    display: flex;
    flex-direction: column;
    padding: 10px;
    background-color: #ffffff;
}

.header {
    display: flex;
    align-items: center;
    padding: 10px;
    font-size: 1.2em;
}

.imagem {
    flex: 1;
    display: flex;
    align-items: center;
    justify-content: center;
    background-color: #f5f5f5;
}

.messages {
    display: flex;
    flex-direction: column;
    gap: 10px;
    padding: 10px;
    background-color: rgba(200, 200, 255, 0.5);
    border-radius: 5px;
    margin-top: 10px;
}

.message,
.my-message {
    background-color: rgba(200, 200, 255, 0.3);
    padding: 10px;
    border-radius: 4px;
}

.suggestions {
    display: flex;
    gap: 10px;
    padding: 10px;
    background-color: rgba(255, 255, 200, 0.5);
    justify-content: center;
}

.suggestion {
    padding: 5px 10px;
    background-color: yellow;
    border: none;
    border-radius: 3px;
    cursor: pointer;
}

.input {
    display: flex;
    justify-content: center;
    padding: 10px;
    background-color: #ddd;
}

input {
    width: 100%;
    padding: 10px;
    border-radius: 5px;
    border: 1px solid #ccc;
}

@media (min-width: 768px) {
    .body {
        flex-direction: row;
    }

    .content {
        padding: 20px;
    }

    .padding {
        width: 10%;
    }

    .header {
        font-size: 1.5em;
    }

    .messages {
        width: 30%;
        align-self: flex-end;
        position: absolute;
        top: 20px;
        right: 10px;
    }
}

@media (min-width: 1024px) {
    .padding {
        width: 15%;
    }

    .suggestions {
        justify-content: flex-start;
    }

    .input {
        padding: 20px;
    }
}
</style>