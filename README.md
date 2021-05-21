<h1 align="center">Teste Mova Front end</h1>

### Pré-requisitos

Antes de começar, você vai precisar ter instalado em sua máquina as seguintes ferramentas:
[Git](https://git-scm.com), [Node.js](https://nodejs.org/en/). 
Além disto é bom ter um editor para trabalhar com o código como [VSCode](https://code.visualstudio.com/)

### 🎲 Rodando o Projeto (Servidor)

```bash
# Clone este repositório
$ git clone <https://github.com/ycarolehmkuhl/bandeiras-move-frontend>

# Acesse a pasta do projeto no terminal/cmd
$ cd bandeiras-move-frontend

# Instale as dependências
$ npm install

# Executando o projeto
$ npm run serve

# O servidor inciará na porta:8081 - acesse

http://localhost:8081/
```
# ✅ Funções
- [x] Selecionar um tipo de filtro: Região, Capital, Lingua, País, Código de ligação. Default País. Esse select define as propriedades do segundo select e a url da requisição;
- [x] Pesquisar pela capital( https://restcountries.eu/rest/v2/capital/{capital} ) exibir apenas a bandeira do país com essa capital;
- [x] Pesquisar pela capital e exibir apenas a bandeira do país com essa capital 
- [x] Pesquisar pela lingua( https://restcountries.eu/rest/v2/lang/{et} ) exibir as bandeiras de todos os países com essa lingua. Paginar de 10 em 10;
- [x] Pesquisar por um país, exibir apenas a bandeira desse país(https://restcountries.eu/rest/v2/alpha/{currency});
- [x] Ao clicar na bandeira abre uma nova página(Tela 2) com mais informações do país( https://restcountries.eu/rest/v2/alpha/{currency} ):
    - imagem da bandeira(flag)
    - name
    - capital
    - região -> Ao clicar na região o usuário deve ser redirecionado para a tela 1 com essa região selecionada
    - subregião
    - população;
    - linguagens;
    - Lista com as bandeiras dos vizinhos(borders). Essas bandeiras são clicáveis e leva para uma nova página com detalhes do país clicado.

### 🛠 Tecnologias

As seguintes ferramentas foram usadas na construção do projeto:

- [Vue.JS](https://vuejs.org/)

# Autor
## ☕ Ycaro de S. Lehmkuhl ☕
- 😎 Frontend Developer 
- 📞 (47) 9.9601-1022
- 📞 (48) 9.8409-9301
- ✉️ E-mail - ycaro.lehmkuhl@gmail.com
- 🔗 [Linkedin](https://www.linkedin.com/in/ycaro-de-souza-lehmkuhl-4104924a/)
- 🔗 [Site Pessoal](https://ycarosl.com)

