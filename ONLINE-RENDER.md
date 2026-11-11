# Colocar a V3 online para testar, sem plano pago

O pacote foi preparado para um Web Service gratuito no Render. O jogo ainda NÃO está hospedado no Render: os passos abaixo precisam ser feitos na sua conta. Não é necessário instalar Node.js no seu computador para esse caminho.

## 1. Criar sua conta

Entre em https://dashboard.render.com/register e use sua conta GitHub. Use a opção gratuita de conta/workspace. O plano da conta e o plano do servidor são coisas separadas: o servidor também precisa estar marcado **Free**.

## 2. Colocar os arquivos no GitHub

1. Baixe e extraia `we-need-to-rob-v3-online.zip`.
2. No GitHub, clique no sinal **+**, depois **New repository**.
3. Nome: `we-need-to-rob-v3`. Escolha **Private** para manter o código privado. Clique em **Create repository**.
4. Clique em **uploading an existing file** (ou **Add file → Upload files**).
5. Abra a pasta extraída. Arraste **o conteúdo de dentro dela**, incluindo as pastas, para a página do GitHub. Não envie o ZIP nem a pasta externa inteira.
6. Clique em **Commit changes**. Na página principal devem aparecer `package.json`, `render.yaml`, `server` e `dist` diretamente.

Não envie a pasta `node_modules` do pacote LAN antigo. Este pacote online é menor e o Render instala as dependências automaticamente.

## 3. Criar o servidor no Render

1. No painel do Render, clique em **New + → Web Service**. Não escolha Static Site.
2. Conecte o GitHub e permita acesso ao repositório `we-need-to-rob-v3`. Selecione esse repositório.
3. Preencha:

| Campo | Valor |
|---|---|
| Name | `we-need-to-rob-v3` (ou outro nome disponível) |
| Language / Runtime | `Node` |
| Branch | `main` |
| Root Directory | deixe vazio |
| Build Command | `npm ci --omit=dev` |
| Start Command | `npm start` |
| Instance Type | **Free — $0** |

4. Em Environment Variables, adicione `NODE_VERSION` com valor `22` e `MAX_ROOMS` com valor `1`. Em Advanced, o Health Check Path pode ser `/api/health`. A porta é escolhida automaticamente pelo Render.
5. Confira **Free / $0** e clique em **Deploy Web Service**. Aguarde aparecer **Live**.
6. Abra o endereço HTTPS mostrado pelo Render, terminado em `.onrender.com`. Todos os jogadores usam esse mesmo link, mesmo em redes diferentes.
7. Crie uma sala e compartilhe o código com seus amigos. Para cooperativo, ambos escolhem ladrões; para assimétrico são três pessoas.

O arquivo `render.yaml` também permite configurar por **New → Blueprint**, escolhendo o repositório. Ele já declara o plano gratuito e uma única sala por vez. Nenhum banco de dados é necessário.

## Para não contratar nada sem querer

Escolha somente **Free**. Não habilite upgrade automático ou outros serviços pagos. Se a página pedir cartão ou mostrar cobrança, pare nesse ponto e confira a tela antes de continuar. O serviço tem limites mensais; não prometemos disponibilidade ilimitada. Sem meio de pagamento, ao atingir certos limites o Render pode suspender o serviço em vez de cobrar.

## O que esperar no teste

- Após 15 minutos sem tráfego, o serviço gratuito adormece. A primeira pessoa a abrir o link pode esperar cerca de um minuto.
- Reiniciar/reimplantar/adormecer apaga as salas em memória; basta criar outra sala.
- Há uma sala simultânea no preset gratuito para limitar consumo. Não se trata de uma medição de capacidade: fluidez/latência precisam ser avaliadas depois de publicar.
- Mantenha uma única instância. O protótipo não compartilha estado entre vários servidores.
- Não precisa abrir o `.bat`, manter seu PC como servidor, informar IP nem configurar roteador.

Documentação consultada: https://render.com/docs/free e https://render.com/docs/websocket . As condições gratuitas podem mudar. Configuração e testes locais não confirmam uma publicação no Render.
