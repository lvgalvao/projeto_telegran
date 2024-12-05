# Documentação do Projeto

## Descrição

Este projeto é um script em Python que extrai o preço de um produto da página do Mercado Livre e envia essa informação para um chat do Telegram. Utiliza as bibliotecas `requests`, `BeautifulSoup` e `python-telegram-bot` para realizar a requisição HTTP, fazer o parsing do HTML e enviar mensagens, respectivamente.

## Pré-requisitos

Antes de executar o script, você precisa instalar as seguintes bibliotecas:

```bash
pip install requests beautifulsoup4 python-telegram-bot
```

## Configuração

1. **URL do Produto**: Altere a variável `url` para o link do produto que você deseja monitorar.
2. **Token do Telegram**: Substitua o valor da variável `token` pelo seu token de bot do Telegram.
3. **Chat ID**: Substitua o valor da variável `chat_id` pelo ID do chat onde você deseja enviar a mensagem.

## Código

```python
import requests
from bs4 import BeautifulSoup
from telegram import Bot  # Importando a classe Bot da biblioteca python-telegram-bot
import asyncio  # Importando asyncio para suporte assíncrono

# URL do produto
url = "https://www.mercadolivre.com.br/48-un-mini-pokebola-com-miniaturas-pokemon-lacrada-surpresa/p/MLB23511196#reco_item_pos=0&reco_backend=item_decorator&reco_backend_type=function&reco_client=home_items-decorator-legacy&reco_id=17517297-bef3-495b-9d46-7cc4f705ff91&reco_model=&c_id=/home/navigation-recommendations-seed/element&c_uid=6877e908-08cf-44e6-8af7-684f53807a57&da_id=navigation&da_position=0&id_origin=/home/dynamic_access&da_sort_algorithm=ranker"

# Token e chat ID do Telegram
token = "7817019045:AAHiV1n1xLTPf9ZK0sD-GJlO8ewPDYgpviI"
chat_id = "6852371789"
bot = Bot(token)  # Criando uma instância do Bot

async def main():
    # Fazendo a requisição
    response = requests.get(url)

    # Verificando se a requisição foi bem-sucedida
    if response.status_code == 200:
        # Criando o objeto BeautifulSoup
        soup = BeautifulSoup(response.text, 'html.parser')
        
        # Selecionando o preço usando o seletor CSS fornecido
        price_selector = "#ui-pdp-main-container > div.ui-pdp-container__col.col-3.ui-pdp-container--column-center.pb-16 > div > div.ui-pdp-container__row.ui-pdp-with--separator--fluid.ui-pdp-with--separator--40-24 > div.ui-pdp-container__col.col-2.mr-24.mt-8 > div.ui-pdp-price.mt-16.ui-pdp-price--size-large > div.ui-pdp-price__main-container > div.ui-pdp-price__second-line > span:nth-child(1) > span > span.andes-money-amount__fraction"
        
        # Encontrando o elemento do preço
        price_element = soup.select_one(price_selector)
        
        if price_element:
            # Extraindo e imprimindo o preço
            price = price_element.text
            print(f"O preço do produto é: R${price}")
            
            # Enviando o preço para o Telegram
            await bot.send_message(chat_id=chat_id, text=f"O preço do produto é: R${price}")  # Enviando a mensagem
        else:
            print("Elemento de preço não encontrado.")
    else:
        print(f"Erro ao acessar a página: {response.status_code}")

# Executando a função principal
asyncio.run(main())
```

## Execução

Para executar o script, utilize o seguinte comando:

```bash
python main.py
```

## Observações

- Certifique-se de que o bot do Telegram tenha permissão para enviar mensagens no chat especificado.
- O seletor CSS utilizado para encontrar o preço pode precisar de ajustes se a estrutura da página do Mercado Livre mudar.# projeto_telegran
