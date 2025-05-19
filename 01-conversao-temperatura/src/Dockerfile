# Especifica a imagem base a ser usada. Neste caso, é a imagem oficial do Node.js na versão 20.19.0.
FROM node:20.19.0 

# Define o diretório de trabalho dentro do contêiner. Todos os comandos subsequentes serão executados neste diretório.
WORKDIR /app

# Copia os arquivos package.json e package-lock.json (se existirem) do diretório local para o diretório de trabalho no contêiner.
COPY ./package*.json ./

# Executa o comando `npm install` para instalar as dependências listadas no package.json.
# Devo utilizar o RUN sempre que quero executar um comando na construção da imagem.
RUN npm install

# Copia todos os arquivos e diretórios do projeto local para o diretório de trabalho no contêiner.
COPY . . 

# Define o comando padrão que será executado quando o contêiner for iniciado.
# Neste caso, ele executa o arquivo `server.js` usando o Node.js.
# O CMD é o comando que será executado quando o contêiner for iniciado.
# O CMD deve ser o último comando no Dockerfile.
CMD ["node", "server.js"]