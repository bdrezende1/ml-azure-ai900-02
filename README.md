# ml-azure-ai900-02
Praticar a criação de reconhecimento facial, identificação de dados em documentos e também o reconhecimento de elementos em imagens.
---------------------------------------------------------------------------------------------

# Laboratório de Análise de Imagens

## 1. Visão Geral
Neste laboratório, vou aprender a usar serviços de análise de imagem da Azure para identificar objetos e características dentro de uma imagem. É uma excelente introdução ao uso de inteligência artificial para interpretar imagens.

## 2. Criando um Recurso no Azure
Primeiro, preciso criar um recurso de Serviços Cognitivos no Azure:
1. Acesso o [Portal do Azure](https://portal.azure.com/).
2. Clico em **Criar um recurso** e procuro por **Serviços Cognitivos**.
3. Seleciono **Análise de Imagem** e clico em **Criar**.
4. Preencho as informações necessárias como assinatura, grupo de recursos, nome do recurso e região.
5. Clico em **Revisar + criar** e depois em **Criar**.

## 3. Obtendo a Chave e o Ponto de Extremidade
Após criar o recurso, preciso da chave e do ponto de extremidade para acessar o serviço:
1. Navego até o recurso criado.
2. Clico em **Chaves e ponto de extremidade** no menu lateral.
3. Copio a chave primária e o ponto de extremidade. Vou precisar disso para minhas requisições.

## 4. Fazendo uma Análise de Imagem
Agora, vou fazer uma análise de uma imagem usando a API de Análise de Imagem:
1. Faço uma requisição HTTP POST para a URL do ponto de extremidade, anexando a chave de API nos cabeçalhos.
2. No corpo da requisição, incluo a URL da imagem que desejo analisar.

Aqui está um exemplo de código em Python para fazer isso:

```python
import requests

subscription_key = "YOUR_SUBSCRIPTION_KEY"
endpoint = "YOUR_ENDPOINT"

analyze_url = endpoint + "vision/v3.0/analyze"

headers = {'Ocp-Apim-Subscription-Key': subscription_key}
params = {'visualFeatures': 'Categories,Description,Color'}
image_url = "IMAGE_URL"

data = {'url': image_url}
response = requests.post(analyze_url, headers=headers, params=params, json=data)
response.raise_for_status()

analysis = response.json()

print("Descrição da imagem: ", analysis["description"]["captions"][0]["text"])
```

### Resultado
A resposta da API incluirá informações sobre a imagem, como descrição, categorias, e cores predominantes.

---

# Laboratório de Detecção de Faces

## 1. Visão Geral
Neste laboratório, vou usar os serviços de detecção de faces da Azure para identificar rostos em uma imagem, além de extrair características como emoções, idade e gênero.

## 2. Criando um Recurso de Detecção de Faces
1. Acesso o [Portal do Azure](https://portal.azure.com/).
2. Crio um novo recurso de **Serviços Cognitivos**.
3. Seleciono **Detecção de Faces** e clico em **Criar**.
4. Preencho as informações e clico em **Revisar + criar** e depois em **Criar**.

## 3. Obtendo a Chave e o Ponto de Extremidade
1. Navego até o recurso criado.
2. Copio a chave primária e o ponto de extremidade.

## 4. Detectando Faces em uma Imagem
Podemos fazer uma requisição para detectar faces:

```python
import requests

subscription_key = "YOUR_SUBSCRIPTION_KEY"
endpoint = "YOUR_ENDPOINT"

face_api_url = endpoint + "face/v1.0/detect"

headers = {'Ocp-Apim-Subscription-Key': subscription_key, 'Content-Type': 'application/json'}
params = {
    'returnFaceId': 'true',
    'returnFaceLandmarks': 'false',
    'returnFaceAttributes': 'age,gender,smile,facialHair,glasses,emotion'
}
image_url = "IMAGE_URL"

data = {'url': image_url}
response = requests.post(face_api_url, headers=headers, params=params, json=data)
response.raise_for_status()

faces = response.json()

for face in faces:
    print("Face ID: ", face["faceId"])
    print("Idade: ", face["faceAttributes"]["age"])
    print("Gênero: ", face["faceAttributes"]["gender"])
    print("Emoções: ", face["faceAttributes"]["emotion"])
```

### Resultado
A resposta da API fornecerá detalhes sobre cada rosto detectado, incluindo idade, gênero, e emoções.

---

# Laboratório de Reconhecimento Óptico de Caracteres (OCR)

## 1. Visão Geral
Neste laboratório, vou usar o serviço de OCR da Azure para extrair texto de imagens. Este é um recurso útil para digitalizar documentos ou extrair informações de imagens.

## 2. Criando um Recurso de OCR
1. Acesso o [Portal do Azure](https://portal.azure.com/).
2. Crio um novo recurso de **Serviços Cognitivos**.
3. Seleciono **OCR** e clico em **Criar**.
4. Preencho as informações e clico em **Revisar + criar** e depois em **Criar**.

## 3. Obtendo a Chave e o Ponto de Extremidade
1. Navego até o recurso criado.
2. Copio a chave primária e o ponto de extremidade.

## 4. Realizando OCR em uma Imagem
Podemos fazer uma requisição para extrair texto:

```python
import requests

subscription_key = "YOUR_SUBSCRIPTION_KEY"
endpoint = "YOUR_ENDPOINT"

ocr_url = endpoint + "vision/v3.0/ocr"

headers = {'Ocp-Apim-Subscription-Key': subscription_key}
image_url = "IMAGE_URL"

params = {'language': 'unk', 'detectOrientation': 'true'}
data = {'url': image_url}
response = requests.post(ocr_url, headers=headers, params=params, json=data)
response.raise_for_status()

ocr_results = response.json()

for region in ocr_results["regions"]:
    for line in region["lines"]:
        line_text = " ".join([word["text"] for word in line["words"]])
        print(line_text)
```

### Resultado
A resposta da API fornecerá o texto extraído da imagem, permitindo que eu possa utilizá-lo em outras aplicações.

---
