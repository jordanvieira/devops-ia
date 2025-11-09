# Estágio 1: Build
FROM python:3.11-slim as builder

# Define o diretório de trabalho
WORKDIR /app

# Instala dependências do sistema
RUN apt-get update && apt-get install -y --no-install-recommends gcc

# Instala dependências Python
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Estágio 2: Final
FROM python:3.11-slim

# Define o diretório de trabalho
WORKDIR /app

# Cria um usuário não-root
RUN useradd --create-home appuser
USER appuser

# Copia as dependências instaladas do estágio de build
COPY --from=builder /usr/local/lib/python3.11/site-packages /usr/local/lib/python3.11/site-packages
COPY --from=builder /usr/local/bin /usr/local/bin

# Copia o código da aplicação
COPY . .

# Expõe a porta que a aplicação vai rodar
EXPOSE 8000

# Comando para iniciar a aplicação com Gunicorn
CMD ["gunicorn", "--bind", "0.0.0.0:8000", "main:app"]
