# Segmentação de Bovinos com YOLOv11

Este projeto aplica técnicas de Visão Computacional e Deep Learning para a **segmentação de instâncias de gado** (bovinos) em imagens. O foco principal é a implementação da arquitetura moderna **YOLOv11** e a validação dos resultados contra um padrão-ouro (Ground Truth) gerado por um Zootecnista especialista em avaliação morfológica de bovinos.

![Python](https://img.shields.io/badge/Python-3.X-blue)
![YOLO](https://img.shields.io/badge/YOLO-v11-green)
![OpenCV](https://img.shields.io/badge/OpenCV-Computer%20Vision-red)

## Sobre o Projeto

A segmentação precisa de animais é um passo fundamental para aplicações na pecuária de precisão, como estimativa de peso, avaliação de condição corporal e monitoramento de saúde.
Diferente da detecção de objetos (que apenas encontra a caixa delimitadora), este projeto visa identificar a **silhueta exata (máscara)** de cada animal utilizando o estado da arte em modelos de segmentação.

### Destaques
* **Arquitetura de Ponta:** Utilização do modelo `YOLOv11-seg` (via framework Ultralytics), focado em eficiência e precisão.
* **Validação Rigorosa:** Implementação de um pipeline de validação customizado que compara as predições da IA com máscaras criadas manualmente por um **Zootecnista (Especialista)**.
* **Métricas Reais:** Avaliação baseada em métricas de classificação de pixels (IoU, Dice, Recall) em vez de métricas de regressão.

---

## Metodologia

### 1. Dataset e Treinamento
O dataset foi gerenciado via Roboflow e o treinamento realizado no Google Colab utilizando GPUs.
* **Framework:** Ultralytics.
* **Tarefa:** Segmentação de Instâncias (`task=segment`).
* **Modelo:** `yolo11n-seg.pt` (Versão Nano para alta eficiência e velocidade).

### 2. Validação Customizada (O Diferencial)
As métricas padrão de treinamento (mAP) muitas vezes são otimistas demais em datasets pequenos. Para garantir a robustez, desenvolvemos um script Python para validação externa:

1.  **Ground Truth:** Máscaras binárias geradas a partir de anotações manuais de um especialista (imagens PNG com fundo transparente).
2.  **Predição:** Extração das máscaras brutas do modelo YOLOv11 treinado.
3.  **Padronização:** Redimensionamento inteligente (com *padding*) para garantir que ambas as máscaras (IA vs. Humano) tenham a mesma escala e proporção (640x640).
4.  **Comparação:** Cálculo pixel-a-pixel das métricas de sobreposição.

---

## Resultados e Métricas

O modelo demonstrou um desempenho robusto e satisfatório na segmentação dos animais. Abaixo estão as definições das métricas utilizadas para avaliar a eficácia:

| Métrica | Definição | Interpretação no Projeto |
| :--- | :--- | :--- |
| **IoU (Intersection over Union)** | Sobreposição entre a máscara predita e a real. | Nossa métrica principal. Indica o quanto a forma do boi prevista "encaixa" na real. |
| **Dice Coefficient (F1)** | Média harmônica entre Precisão e Recall. | Equilibra a avaliação entre contornos precisos e detecção completa. |
| **Recall (Sensibilidade)** | Capacidade de não "esquecer" pixels do boi. | Um Recall alto indica que o modelo detectou quase todo o corpo do animal, sem cortes. |
| **Precision (Precisão)** | Pureza da máscara (evitar fundo). | Indica que o modelo não está confundindo o pasto ou sombras com o animal. |

### Performance Obtida

* **Média Final IoU:** `0,8754`
* **Média Final Recall:** `0,9111`
* **Média Final Dice:** `0,9033`

> **Análise:** As análises indicam que o modelo YOLOv11 foi capaz de generalizar bem. O alto nível de Recall demonstra que o sistema é eficaz em localizar o animal por completo, enquanto o IoU reflete uma boa aderência ao contorno complexo desenhado pelo especialista.

---

## Visualização

Comparativo visual entre a anotação do especialista e a predição do modelo:

<img width="1152" height="578" alt="image" src="https://github.com/user-attachments/assets/ffb0694e-a336-4386-94dd-031b18999bfe" />

---

## Tecnologias Utilizadas
- [Ultralytics YOLO](https://github.com/ultralytics/ultralytics)
- [Google Colab](https://colab.research.google.com)
- [Roboflow](https://roboflow.com)
- Python (NumPy, Pandas, Matplotlib, OpenCV)

## Autor
[George Henrique Almeida da Silva] - Engenharia da Computação - IFMT






