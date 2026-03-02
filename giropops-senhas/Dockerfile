# ==============================================================================
# Estágio 1: Builder Python (Apenas dependências do Flask/Redis)
# ==============================================================================
FROM cgr.dev/chainguard/python:latest-dev AS builder

WORKDIR /app

COPY requirements.txt .

# Instala as dependências no diretório do usuário
RUN pip install \
    --user \
    --no-cache-dir \
    -r requirements.txt

# ==============================================================================
# Estágio 2: Imagem Final (Runtime)
# ==============================================================================
FROM cgr.dev/chainguard/python:latest

WORKDIR /app

# Configura o PATH para encontrar as libs instaladas
ENV PATH="/home/nonroot/.local/bin:$PATH"
ENV PYTHONPATH="/home/nonroot/.local/lib/python3.12/site-packages"

# 1. Copia as bibliotecas instaladas do Estágio 1
COPY --from=builder /home/nonroot/.local /home/nonroot/.local

# 2. Copia o código da aplicação
COPY app.py .
COPY templates ./templates

# 3. COPIA A PASTA STATIC DA SUA MÁQUINA
# Como você já gerou o CSS localmente, copiamos tudo direto.
COPY static ./static

EXPOSE 5000

ENTRYPOINT ["python", "app.py"]