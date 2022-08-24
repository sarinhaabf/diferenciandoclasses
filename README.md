# diferenciandoclasses
Dados sobre a possibilidade de ataque cardíaco

# importando bibliotecas
import seaborn as sns
from mpl_toolkits.mplot3d import Axes3D
import matplotlib.pyplot as plt
from matplotlib.lines import Line2D
import numpy as np
import pandas as pd
from matplotlib import cm
from IPython.display import clear_output

# Carregar o conjunto de dados Heart Attack Possibility
data = pd.read_csv('heart.csv')

#Atribuir os nomes corretos a cada variável
data.columns = [
    'age',
    'sex',
    'cp',
    'trestbps',
    'chol',
    'fbs',
    'restecg',
    'thalach',
    'exang',
    'oldspeak',
    'slope',
    'ca',
    'thal',
    'target',
]

data.head(20)

#frequência por probabilidade
data['target'].value_counts()

sns.pairplot(
    data,
    hue="target",
    vars=[
        d for d in data.columns if d!='target'
    ]
)

#cria a janela da figura
fig = plt.figure()

#carrega os eixos criando um a mais se necessário
ax = fig.gca(projection='3d')

#cria lista de cor considerando qualidade
colors = data['target'].map({0:'g',1:'r'}).values
ax.scatter(
    data['cp'],
    data['chol'],
    data['thalach'],
    c=colors
)
plt.title("target")
plt.xlabel('cp')
plt.ylabel('chol')
ax.set_zlabel('thalach')
legend_elements = [
    Line2D([0], [0],
        marker='o',
        color='w',
        label='not likely',
        markerfacecolor='g',
        markersize=10
    ),
    Line2D(
        [0], [0],
        marker='o',
        color='w',
        label='likely',
        markerfacecolor='r',
        markersize=10
    )
]
ax.legend(handles=legend_elements, loc='best')


#ajusta e mostra o gráfico
plt.tight_layout()
plt.show()

# filtro: 4 variáveis
data = data[[
    'target',
    'cp',
    'chol',
    'thalach'
]]
