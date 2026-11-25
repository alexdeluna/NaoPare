# NÃO PARE — Gestão do Box V1.3 (Firebase)

PWA de gestão do NÃO PARE, com autenticação administrativa e formulário público de solicitação de matrícula.

## Funcionalidades V1.3
- Login administrativo via Firebase Authentication (e-mail/senha).
- Acesso de administrador limitado ao UID configurado no projeto.
- Cadastro público por link `?cadastro=1`.
- Solicitações armazenadas na coleção `registrationRequests`, inicialmente pendentes.
- Painel administrativo para revisar, aprovar ou recusar solicitações.
- Ao aprovar, cria o aluno no cadastro local sincronizado no documento Firestore `adminData/main`.
- Persistência/sincronização da base principal no Firestore para o usuário administrador.
- Demais recursos da V1.2 (alunos, pagamentos, Pix, mensagens, relatórios e exportações).

## Firebase já configurado no código
Project ID: `box-controle`
Firebase Auth: e-mail/senha
Admin UID: `hZKvrzEeUPMwdJpPGmgS77e8hYL2`

A Firebase API key do web app não é uma senha e é necessária no cliente. A senha de administrador NÃO está armazenada no projeto.

## Passos para publicar
1. Firebase Console → Authentication → Sign-in method → habilite Email/Password.
2. Confirme que o usuário administrativo existe e corresponde ao UID indicado.
3. Firebase Console → Firestore Database → Rules, copie o conteúdo de `firestore.rules` e publique.
4. Hospede a pasta completa em Firebase Hosting, GitHub Pages ou outro host HTTPS.
5. Link público do formulário: acrescente `?cadastro=1` ao endereço principal (ex.: `https://SEU-SITE/?cadastro=1`).
6. O link administrativo é o endereço normal, sem parâmetro.

## Segurança
- Visitantes podem somente criar solicitações com status `pending` e campos limitados.
- Visitantes não podem ler solicitações, cadastros ou pagamentos.
- O administrador identificado pelo UID pode ler/escrever `adminData/main` e gerenciar solicitações.
- Não coloque a senha do administrador no código, URL ou repositório.
- Recomenda-se trocar a senha mostrada na conversa antes de usar em produção.

## Observações
O projeto é HTML/CSS/JavaScript puro, sem framework ou bundler. Para o PWA, publicar via HTTPS.

## Limpeza de dados de implementação (temporária)
No Dashboard existe o botão **Limpar dados de implementação**. Ele remove alunos, pagamentos e todas as solicitações de matrícula (incluindo já processadas) do Firestore e da base local. Preserva autenticação administrativa, Firebase, configurações Pix, mensagens e demais configurações. Requer confirmação e digitação de `APAGAR TESTES`. Remova este recurso antes da utilização definitiva do sistema.


V1.3.4: correção defensiva de renderização para evitar Dashboard vazio/congelado quando dados locais ou Firebase têm campos inconsistentes; mensagem de erro visível e log no console.


## V1.3.5 — correção crítica

- Corrigido erro de sintaxe no Dashboard: template literal da área de manutenção tinha um ramo condicional incompleto (`:}`), impedindo o módulo JavaScript de carregar.
- Com o módulo falhando, a tela administrativa e o login ficavam em estado visual incorreto e os botões não funcionavam.
- Atualizado o cache do Service Worker para V1.3.5 e remoção dos caches anteriores na ativação.
- Validado com `node --check app.js`.

## V1.3.6 — Página pública de cadastro
- Página pública redesenhada com identidade visual NÃO PARE, responsiva e mobile-first.
- Formulário com nome, WhatsApp (máscara e validação), nascimento opcional e observações opcionais.
- Botão com estado de envio, proteção contra envio repetido e mensagem de sucesso.
- Texto de privacidade/uso dos dados para contato sobre matrícula.
- Atualização do cache do Service Worker para V1.3.6.


## V1.3.7 — Cobrança e Pix copiáveis
- Na pré-visualização da cobrança, mensagem e código Pix são exibidos em áreas separadas.
- Botão **Copiar somente o código Pix** copia apenas o código/chave Pix configurado.
- Botão **Copiar mensagem** copia o texto completo da cobrança.
- Botão **Abrir WhatsApp** abre a conversa com a mensagem completa, incluindo o Pix conforme o modelo de mensagem.
- Pix destacado em bloco com fonte monoespaçada e quebra de linha, facilitando leitura e cópia no celular.
- Atualizado o cache do PWA para V1.3.7.
- Verificação sintática JavaScript realizada com `node --check app.js`.


## Página pública de pagamento Pix (V1.3.8)

Na pré-visualização de cobrança, clique em **Gerar link público Pix**. O sistema cria um link individual, com token aleatório, que mostra nome, valor, vencimento, código Pix copiável e QR Code. O link pode ser copiado e enviado pelo WhatsApp.

**Antes de testar no Firebase:** publique o arquivo `firestore.rules` atualizado. A regra permite que o administrador crie/gerencie links e que qualquer pessoa leia apenas o documento do link público ativo, pelo token. O público não tem permissão de listar links nem acessar dados administrativos.

O QR Code é gerado no navegador por biblioteca carregada via CDN; a página continua oferecendo o código Pix copia-e-cola caso a biblioteca de QR não carregue.
