<<<<<<< HEAD
# Portfólio

Bem vindos(as) ao meu Portfólio. Pretendo contribuir com estudos de ciência de ciência de dados e machine learning neste portfólio com o passar do tempo.

## Projeto de Precificação de Aluguel

Este projeto tem como objetivo treinar a predição de aluguéis considerando um csv com diversas linhas como esta por exemplo: 

{'id': 2595,
 'nome': 'Skylit Midtown Castle',
 'host_id': 2845,
 'host_name': 'Jennifer',
 'bairro_group': 'Manhattan',
 'bairro': 'Midtown',
 'latitude': 40.75362,
 'longitude': -73.98377,
 'room_type': 'Entire home/apt',
 'price': 225,
 'minimo_noites': 1,
 'numero_de_reviews': 45,
 'ultima_review': '2019-05-21',
 'reviews_por_mes': 0.38,
 'calculado_host_listings_count': 2,
 'disponibilidade_365': 355}

### Requirements

Para instalar os pacotes utilizados neste projeto, navegue no cmd até o seu diretório onde salva os seus scripts e execute pip install -r requirements.txt

### Utilização

Além de arquivo.pkl, exemplo de base de dados em .csv e jupyter notebook com a análise por célula, vou deixar esta pequena seção para auxiliar na aplicação.

import pickle
with open('LH_CD_GABRIELDAUER.pkl', 'rb') as file:
    modelo = pickle.load(file)
import pandas as pd
from sklearn.metrics import r2_score
import sklearn.model_selection as ms
import matplotlib.pyplot as plt

df = pd.read_csv('teste_indicium_precificacao.csv')

df = df.dropna()

percentile_92 = df[['price', 'minimo_noites', 'numero_de_reviews', 'reviews_por_mes', 'calculado_host_listings_count', 'disponibilidade_365']].quantile(0.92)

df_clean = df[(df['price'] <= percentile_92['price']) &
              (df['minimo_noites'] <= percentile_92['minimo_noites']) & 
              (df['numero_de_reviews'] <= percentile_92['numero_de_reviews']) & 
              (df['reviews_por_mes'] <= percentile_92['reviews_por_mes']) & 
              (df['calculado_host_listings_count'] <= percentile_92['calculado_host_listings_count']) & 
              (df['disponibilidade_365'] <= percentile_92['disponibilidade_365'])]

#Tranformação das variáveis qualitativas para variáveis dummy.
bairro_group_dummy = pd.get_dummies(df_clean['bairro_group']).iloc[:, 1:]
room_type_dummy = pd.get_dummies(df_clean['room_type']).iloc[:, 1:]

#Modificação nas colunas para posteriormente facilitar na identificação das variáveis independentes e dependente.
df2 = df_clean.drop(['price', 'id', 'nome', 'host_name', 'host_id', 'latitude', 'longitude', 'ultima_review', 'bairro_group', 'bairro', 'room_type'], axis= 1)
df3 = pd.concat([df2, bairro_group_dummy, room_type_dummy], axis= 1)

#Identificação das variáveis independentes e dependente.
x = df3
y = df_clean['price'].copy()

x_train, x_test, y_train, y_test = ms.train_test_split(x, y, test_size= 0.2, random_state= 0)

#Fit e predição do modelo.
modelo.fit(x_train, y_train)
previsao = modelo.predict(x_train)

#Predição dos dados considerando o modelo criado.
pred_train = modelo.predict(x_train)
pred_test = modelo.predict(x_test)

r_train = r2_score(y_train, pred_train)
r_test = r2_score(y_test, pred_test)

modelo.intercept_

modelo.coef_

print(f'O R² do tratamento do modelo é: {r_train}')
print(f'O R² do controle do modelo é: {r_test}')
print(f'O intercepto do modelo é: {modelo.intercept_}')
print(f'Os coeficientes do modelo são: {modelo.coef_}')

#Gráfico de regressão do modelo
plt.figure(figsize=(10, 6))

#Plotando os valores reais
plt.scatter(y_test, pred_test, color='blue', label='Valores reais')

#Plotando a linha de regressão
plt.plot(y_test, y_test, color='red', label='Linha de regressão')

plt.xlabel('Valores Reais')
plt.ylabel('Valores Previstos')
plt.title('Valores Reais vs Valores Previstos')
plt.legend()

plt.show()
=======
# ps_target
>>>>>>> 3b89c77d1a41b26a08cf6d6d3e216863f06c6d55
