# ResgateJá — Triagem Visual Automatizada de Avarias Veiculares (Applied Computer Vision)

Módulo de Visão Computacional desenvolvido para a **Global Solution FIAP 2026.2 (CitizenTech)** na disciplina de **Applied Computer Vision** (Prof. Paulo Rodrigues).

## Integrantes
* **Márcio Hitoshi Tahyra** — RM552511
* **Jéssica Witzler Costacurta** — RM99068

---

## Sobre o Projeto
O **ResgateJá** é um marketplace digital de assistência veicular sob demanda (*pay-per-use*) que conecta motoristas em emergência diretamente a guincheiros e socorristas próximos, eliminando mensalidades e garantindo transparência no preço fixo e no tempo estimado de chegada (ETA) antes da confirmação do chamado.

O componente de **Applied Computer Vision** atua na etapa de triagem do sinistro: a partir de uma foto ou captura de câmera do veículo avariado, um modelo customizado **YOLOv8m** identifica automaticamente o tipo e a severidade do dano, definindo o modal de atendimento adequado (borracheiro móvel, plataforma com vedação ou plataforma hidráulica) e calculando o valor fixo transparente em tempo real.

### Classes Detectadas
* `0: pneu_furado` — Pneu murcho ou rasgado (Severidade Moderada)
* `1: vidro_quebrado` — Vidro ou para-brisa trincado/danificado (Severidade Alta)
* `2: colisao_lataria` — Colisão ou avaria estrutural na carroceria (Severidade Crítica)

---

## Arquitetura e Métricas de Desempenho
O modelo foi treinado de forma customizada (`fine-tuning`) a partir da arquitetura **Ultralytics YOLOv8m** (92 camadas fusionadas e 25,8 milhões de parâmetros) por **70 épocas** (`imgsz=640`, `batch=16`, `cos_lr=True`, `erasing=0.0`) sobre um dataset unificado e balanceado de **900 imagens** (300 por classe, divididas em 70% treino, 20% validação e 10% teste).

| Classe | Imagens (Val) | Instâncias | Precision | Recall | mAP@50 | mAP@50-95 |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| **Geral (Todas as classes)** | **180** | **196** | **86,0%** | **76,7%** | **80,3%** | **59,7%** |
| `pneu_furado` | 60 | 60 | 96,5% | 100,0% | 99,5% | 93,4% |
| `colisao_lataria` | 60 | 61 | 76,1% | 67,9% | 71,9% | 45,9% |
| `vidro_quebrado` | 60 | 75 | 85,4% | 62,3% | 69,6% | 39,7% |

* **Tempo médio de inferência:** `23,1 ms` por imagem (~43 FPS).

---

## Estrutura do Repositório
```text
├── notebooks/
│   └── GS_visao_comp.ipynb    # Pipeline completo: coleta, unificação, treino, validação e interface Gradio
├── weights/
│   └── best.pt                # Pesos treinados do modelo YOLOv8m customizado
├── app.py                     # Aplicação web interativa (MVP) em Gradio + OpenCV
├── requirements.txt           # Dependências do projeto
└── README.md                  # Documentação técnica
```

---

## Como Executar Localmente

1. **Clone o repositório e instale as dependências:**
   ```bash
   git clone <URL_DO_SEU_REPOSITORIO>
   cd <NOME_DO_REPOSITORIO>
   pip install -r requirements.txt
   ```

2. **Execute a interface web do MVP:**
   ```bash
   python app.py
   ```
   A aplicação estará disponível localmente em `http://127.0.0.1:7860` (e gerará um link público temporário via Gradio) permitindo envio de imagens ou captura via webcam em tempo real.

---

## Links de Entrega
* **Vídeo Demonstrativo (YouTube):** https://youtu.be/bGGc1KvUiDA
* **ODS Conectados:** ODS 9 (Indústria, Inovação e Infraestrutura) e ODS 11 (Cidades e Comunidades Sustentáveis)
