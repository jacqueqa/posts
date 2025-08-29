# Desafio de Testes em API Pet Shop: Como Meu Cérebro Funciona

** Olá, mundo, há quanto tempo :) **

Bem, estou de volta para trazer algumas informações legais para novos QAs em sua jornada ou, por que não, para QAs que já estão no mercado há muito tempo e precisam de um conteúdo novo.  

Veja, não estou aqui para dizer que QA está morrendo e que a IA vai roubar nossos empregos, nem para ensinar como fazer o seu trabalho. Este artigo é apenas a minha mente compartilhando conhecimento para trazer clareza à comunidade.  

Então, vamos ao que realmente importa:  

---

## O Projeto

Como QA híbrido, às vezes você passa a maior parte do seu tempo de trabalho fazendo coisas como: escrever testes, estudar requisitos, reuniões, reuniões, reuniões, testes manuais e, dependendo do projeto em que você está, pode acabar lidando com automação — seja para começar, manter, gerenciar, etc.  

Não sou o melhor analista de QA, mas sou um baita engenheiro de QA e, por causa disso, adoro otimizar processos automatizando tarefas, entendendo a complexidade do código, testando coisas novas e, adivinha só... programando!  

Precisei de um tempo para refletir sobre minha carreira e decidi tentar algo novo para o meu “portfólio”. Então, fui desafiada a pegar uma API pública e testá-la. Simples assim: *“Você tem essa documentação da API no Swagger, o que você vai testar, como vai testar e, se possível, o que vai automatizar?”*  

E foi assim que o projeto nasceu:  

---

## 1. Criando a Estrutura do Projeto

Então, eu era uma garota com um *swagger* e um sonho, certo? Errado... Como diabos o meu cérebro com TDAH ia começar qualquer coisa sem requisitos de produto bem definidos?  

Esse foi o primeiro desafio: eu precisava parar de ser tão dependente de documentos de requisitos. Usei o código (a API) como fonte da verdade.  

A documentação da API é: [Petstore Swagger](https://petstore.swagger.io/) e, como você pode ver, temos muitas requisições que podem ser testadas diretamente na página. Além disso, eles já disponibilizam dados de teste caso você queira validar as ações.  

Você pode me perguntar: *“Ok, e agora? Você tem um monte de endpoints; vai verificar todos?”*  

Eu poderia, mas isso seria contraproducente e não teria nenhum desafio.  

E é por isso que precisei começar a pensar de forma mais voltada ao negócio. Pense comigo: em uma pet shop real, quais seriam os cenários que acontecem diariamente lá? Seguindo os endpoints disponíveis na documentação, imaginei estes cenários:  

- O gerente recebe os filhotes/gatinhos e cria seus registros.  
- O gerente atualiza os dados desses filhotes/gatinhos (se estão disponíveis, reservados ou vendidos).  
- O consumidor quer um pet e consulta a loja para verificar os disponíveis.  
- O consumidor seleciona e compra um pet na loja (essa ação gera um pedido).  

Com isso, ficou fácil criar casos de teste para cada cenário, usando um ou mais endpoints e seus métodos para alcançar o objetivo.  

---

## 2. Casos de Teste

Para esse cérebro com TDAH aqui, essa é a fase “chata”. Precisa ser feita? Sim... Mas é chata...  

Já que tínhamos os cenários e queríamos automatizar um fluxo para eles, precisávamos dos casos de teste. E não pense que só porque eu acho entediante, eu não faço ou faço de qualquer jeito. Pelo contrário: esses casos de teste seriam meu mapa para a automação depois.  

Veja, você pode escrever milhares de casos de teste e investir em planos e ciclos de teste, mas será uma perda de tempo se esses casos não forem úteis ou se já estiverem descritos em um documento de requisitos.  

Eles podem acabar sendo contraproducentes ou inúteis em certas situações, como:  

- Testando requisitos que não são de produto  
- Requisitos que mudam constantemente  
- Código que já está obsoleto ou em refatoração  
- UI simples sem lógica de negócio  
- Comportamento não determinístico ou imprevisível  

Precisamos garantir que nossos testes estejam relacionados à lógica de negócio e ao fluxo.  
E não se engane achando que testes simples não têm valor só porque são: simples.  
Se eles cumprem o que precisam cumprir, não há necessidade de complicar.  

---

## 3. Iniciando a Automação

Depois de criar toda a documentação de testes (casos de teste, dados de teste e blá-blá-blá), o foco foi escolher um framework para automatizá-los.  
Eu estava livre para escolher qualquer ferramenta que quisesse, mas, como eu sabia que as pessoas que me pediram isso trabalhavam com **Cypress 🙂**, a escolha lógica foi **Cypress 😋**.  

Para começar a automação do zero, eu precisava decidir quais resultados seriam relevantes para quem iria receber. Veja, não era só sobre o meu ego aqui, mas também sobre o que realmente precisamos evitar: **overengineering, sobrecarga de trabalho e sensação de estar sobrecarregado**.  
Usei a ferramenta **GitHub Project** para anotar como eu planejava as coisas, o que me ajudou muito, porque consegui manter tudo sob controle e documentado.  

### Minha lista de tarefas para automação foi:

- Instalar o framework (Cypress)  
- Criar um pipeline com **cron diário** (por exemplo, um *sanity check* em produção; se fosse uma empresa de sistema de saúde, seria importante manter os testes rodando diariamente).  
  (E também mostrar que eu sei lidar com arquivos YAML e CI/CD 🤓, mesmo sem me pedirem isso… Mas sou proativa).  
- Criar um relatório (admito, eu queria adicionar o **Allure** aqui, mas fiquei com preguiça e usei só o log bruto do pipeline).  
  > Não façam como eu 😅. Ter um relatório e/ou dashboard com resultados, status e feedback dos testes é importante quando se está começando um projeto de automação do zero ao sucesso.  
- Um *nice-to-have*: webhooks que integrem com **Teams, Slack ou qualquer outro**, para compartilhar o relatório em todos os canais.  

Com tudo isso, a melhor parte para mim estava a caminho: **codar**.  

Para testes de backend, geralmente escrevo como se fosse ser testado manualmente, assim conseguimos uma abordagem mais detalhada.  
E eu **ODEIO a sintaxe Gherkin**. Acho que não precisamos de *given, when, then* para testes de backend. Evitem trabalho desnecessário, pessoal.  

---

## 3.1 Payload e Reuso

Gosto de usar números aleatórios para novos dados (vamos evitar *magic numbers* em uma atualização de API pública, certo?), então criei um comando para gerar um código aleatório e um nome aleatório.  

(Você precisa verificar no Swagger qual é o tipo de dado, quantos caracteres o usuário pode usar e coisas do tipo...)  

O **Payload** é uma variável, então ele também pode ser reutilizado.  
Se o schema mudar e precisarmos adicionar mais chaves e valores ou removê-los, é prático fazer isso.  

---

## 3.2 Validações

E para a **asserção** desse comando, eu fiz assim... Como tentei manter tudo simples e a API do Petstore não me dá muitos detalhes, fiz apenas uma verificação básica do **schema** e da requisição.  
Limpo, simples e eficaz.  

---

## 4. Finalmente: Documentação

Lembre-se: o trabalho do projeto pode ser óbvio para você, mas nunca sabemos quem vai avaliá-lo ou se outras pessoas vão usá-lo ou precisar mantê-lo. Então, uma boa documentação é essencial para evitar mal-entendidos.  

Gosto de pensar que meus passos e meu conhecimento possam ser entendidos e reproduzidos tanto por uma criança de 7 anos quanto por um avô de 90.  

Se é muito difícil para você explicar, então não está bom.  
Eu gosto de manter o princípio **KISS** em todo código em que trabalho...  

---

## Conclusão

Muitos detalhes e muito texto, eu sei, mas não quis pedir para o chatgpt ler minha mente e escrever isso para mim.  
Então este texto é **100% humano**, e mostra mais ou menos como meu cérebro funciona.  

Espero que possa te ajudar também.  

**Obrigada por ler! Tenha um ótimo dia!** ✨  

---
