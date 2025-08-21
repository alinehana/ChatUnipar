# 💬 ChatUnipar

Este projeto foi desenvolvido em sala de aula como atividade prática, conectando-se a um servidor WebSocket por meio de SockJS e túnel via Ngrok.

Interface de chat desenvolvida com JavaScript e Vite, com foco em **UX/UI** (experiência e interface do usuário).  

O objetivo é proporcionar uma comunicação simples e agradável, com visual limpo, responsivo e funcional.

---

## 🎯 Objetivo Geral

A experiência do usuário (UX/UI) é parte fundamental no desenvolvimento web, e um sistema bem-feito visualmente melhora a usabilidade e o engajamento.

## 🔌 Como funciona a conexão no ChatUnipar

O sistema utiliza **WebSocket** com **SockJS** e **STOMP (Simple Text Oriented Messaging Protocol)** para permitir comunicação em tempo real entre os usuários.

### ▶️ Passo a passo da conexão

  1. **Usuário entra no chat**
     - A função `enterChatRoom()` é acionada quando o usuário insere um nome no campo de entrada.
     - Se o nome for válido, chama-se a função `connect()` para iniciar a conexão com o servidor.
  
  2. **Estabelecendo a conexão**
     - `connect()` cria um WebSocket via SockJS apontando para o servidor exposto pelo **Ngrok**:
       ```js
       var socket = new SockJS('https://67e1c8dd9c1a.ngrok-free.app/chat-websocket');
       ```
     - Em seguida, `stompClient = Stomp.over(socket)` inicializa o protocolo STOMP sobre o WebSocket.
  
  3. **Conexão bem-sucedida**
     - Após a conexão, o cliente:
       - **Assina** o canal `/topic/public` para receber mensagens.
       - **Envia** uma mensagem do tipo `"JOIN"` para notificar os demais usuários que alguém entrou no chat:
         ```js
         stompClient.send("/app/addUser", {}, JSON.stringify({ sender: username, type: 'JOIN' }));
         ```
  
  4. **Envio de mensagens**
     - A função `sendMessage()` envia mensagens do tipo `"CHAT"` para o servidor:
       ```js
       stompClient.send("/app/sendMessage", {}, JSON.stringify({
           sender: username,
           content: messageContent,
           type: "CHAT"
       }));
       ```
  5. **Exibição das mensagens**
  - Quando uma nova mensagem chega via `/topic/public`, a função `showMessage()` trata e exibe:
  - `JOIN`: usuário entrou
  - `LEAVE`: usuário saiu
  - `CHAT`: mensagem enviada


## 🚀 Tecnologias Utilizadas

- JavaScript
- HTML5 & CSS3
- Vite
- SockJS (conexão WebSocket)
- Software WebStorm

---

## 🛠 Ferramentas

- Navegadores: Edge / Chrome / Firefox
- Git e GitHub
- WebStorm
- Ngrok (para expor o servidor local)

---

## 📌 Funcionalidades

- ✅ Entrar no chat
- ✅ Enviar e visualizar mensagens em tempo real
- ✅ Layout responsivo
- ✅ Estilo visual moderno, limpo e coerente
