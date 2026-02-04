FROM python:3.9

WORKDIR /app

COPY . .

# Install system dependencies (IMPORTANT: curl + gnupg)
RUN apt-get update && apt-get install -y \
    curl \
    gnupg \
    unixodbc \
    unixodbc-dev \
    ca-certificates

# Add Microsoft signing key (modern way, no apt-key)
RUN curl -sSL https://packages.microsoft.com/keys/microsoft.asc \
    | gpg --dearmor \
    | tee /etc/apt/trusted.gpg.d/microsoft.gpg > /dev/null

# Add Microsoft SQL repo
RUN curl -sSL https://packages.microsoft.com/config/debian/10/prod.list \
    -o /etc/apt/sources.list.d/mssql-release.list

# Install MS ODBC Driver
RUN apt-get update && ACCEPT_EULA=Y apt-get install -y msodbcsql17

# Python deps
RUN pip install --no-cache-dir -r requirements.txt

# Start FastAPI app
CMD ["uvicorn", "app:app", "--host", "0.0.0.0", "--port", "8000"]