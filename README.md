# Classificação de Tumores Cerebrais em Ressonância Magnética (CNN)

Projeto didático de **IA aplicada à saúde**: uma rede neural convolucional (CNN) que classifica ressonâncias magnéticas do cérebro em 4 classes. Criado para a palestra *"Inteligência Artificial Aplicada a Saúde na Prática"* (COMSOLID).

> **Aviso:** este é um projeto educacional. Não é dispositivo médico, não foi validado clinicamente e **não deve ser usado para diagnóstico**.

## O que o projeto faz

- Baixa o dataset público do Kaggle e prepara as imagens (80×80, tons de cinza).
- **Evita vazamento de dados:** remove duplicatas exatas (hash MD5), agrupa imagens parecidas (hash perceptual) e divide treino, validação e teste por grupos.
- Treina uma CNN com aumento de dados (flip, rotação e zoom).
- Avalia com relatório por classe, matriz de confusão e curvas ROC/AUC.
- Mostra **onde o modelo olha** com Grad-CAM.
- Termina com uma demonstração: você passa uma imagem e o modelo devolve a classe.

## Classes

`glioma` · `meningioma` · `pituitária` · `sem tumor`

## Dataset

[Brain Tumor Classification (MRI)](https://www.kaggle.com/datasets/sartajbhuvaji/brain-tumor-classification-mri), de Sartaj Bhuvaji, no Kaggle (3.264 imagens). O download é feito pelo próprio notebook com `kagglehub`; as imagens não estão neste repositório.

## Como rodar (Google Colab, o mais simples)

1. Abra `cerebral_tumor_detection.ipynb` no [Google Colab](https://colab.research.google.com/) (Arquivo > Fazer upload de notebook, ou abra direto pelo GitHub).
2. Se o Kaggle pedir credenciais, crie um token em *Kaggle > Settings > API* e salve em **Secrets** do Colab (ícone de chave) com os nomes `KAGGLE_USERNAME` e `KAGGLE_KEY`.
3. Execute **Ambiente de execução > Executar tudo**, **na ordem** (algumas células dependem das anteriores).
4. Aguarde o treino. Em GPU leva poucos minutos; em CPU demora mais.

## Como rodar localmente

```bash
git clone https://github.com/<seu-usuario>/<repositorio>.git
cd <repositorio>
python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate
pip install tensorflow numpy scipy scikit-learn pillow matplotlib seaborn imagehash kagglehub jupyter
jupyter notebook cerebral_tumor_detection.ipynb
```

Configure o acesso ao Kaggle (variáveis `KAGGLE_USERNAME` e `KAGGLE_KEY`, ou o arquivo `~/.kaggle/kaggle.json`) e execute as células em ordem.

## Parâmetro `TREINAR`

No início do notebook existe `TREINAR = True`.

- `True`: baixa os dados, prepara, treina e avalia (fluxo completo).
- `False`: **ainda não está implementado**. Pula o download e o treino, mas ainda não recarrega os dados e o modelo salvos. Mantenha `True` até essa parte ser feita.

## Arquivos gerados na execução

| Arquivo | Conteúdo |
|---|---|
| `class_names.json` | nomes das classes |
| `splits.npz` | divisão treino/validação/teste |
| `best_model.keras` | melhor modelo (por acurácia de validação) |
| `history.json` | histórico do treino |

## Resultados (nas nossas execuções)

- Acurácia de cerca de **85%** no teste limpo (408 imagens), variando entre 84,8% e 86,8% de uma execução para outra.
- Melhor classe: pituitária. Mais difícil: meningioma.
- Cerca de 243 mil parâmetros.

## Limitações

- Um único dataset, sem validação externa.
- Imagens reduzidas para 80×80, o que perde detalhe.
- O dataset não traz identificação de paciente, então a separação é por similaridade de imagem e não garante que pacientes não se repitam entre treino e teste.
- O modelo classifica a imagem; não localiza nem mede o tumor.

## Referências

- Selvaraju et al. *Grad-CAM: Visual Explanations from Deep Networks via Gradient-based Localization.* ICCV, 2017.
- Bhuvaji, S. et al. *Brain Tumor Classification (MRI).* Kaggle.

## Autor

João Pedro Lima · Instagram [@jplimag.ds](https://instagram.com/jplimag.ds)

