1° passo- Instalar o Mysql workbench e configurar o banco de dados

Código Sql para o Projeto:
-- 1. CRIAÇÃO DO SCHEMA (BANCO DE DADOS)
CREATE SCHEMA IF NOT EXISTS proj_financas DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
USE proj_financas;

-- 2. TABELA USUARIOS (Tabela Mestra/Pai)
-- Guarda o email, que é a chave primária usada como FK em todas as outras tabelas.
CREATE TABLE usuarios (
  email VARCHAR(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci NOT NULL,
  nome VARCHAR(100) NOT NULL,
  senha VARCHAR(100) NOT NULL,
  PRIMARY KEY (email)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

-- 3. TABELA FINANCAS (Orçamento Base - Referencia USUARIOS)
-- Note: Se você usa 'financas' como tabela pai para 'gastos', o email em 'financas'
-- precisa ser uma FK para 'usuarios', e PK para ser referenciado por 'gastos'.
CREATE TABLE financas (
  email_usuario VARCHAR(255) NOT NULL,
  orcamento_inicial DOUBLE DEFAULT NULL,
  orcamento_atual DOUBLE DEFAULT NULL,
  
  PRIMARY KEY (email_usuario),
  
  -- Foreign Key para a tabela mestra (usuarios)
  CONSTRAINT fk_financas_usuario FOREIGN KEY (email_usuario) REFERENCES usuarios (email) 
    ON DELETE CASCADE ON UPDATE CASCADE
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

-- 4. TABELA GASTOS (Depende de FINANCAS/USUARIOS)
CREATE TABLE gastos (
  id INT NOT NULL AUTO_INCREMENT,
  email_usuario VARCHAR(255) NOT NULL,
  tipo ENUM('Fixo','Variável') NOT NULL,
  descricao VARCHAR(100) NOT NULL,
  valor DOUBLE NOT NULL,
  
  PRIMARY KEY (id),
  
  -- Referencia a tabela financas (email_usuario)
  CONSTRAINT fk_gastos_financas FOREIGN KEY (email_usuario) REFERENCES financas (email_usuario) 
    ON DELETE CASCADE ON UPDATE CASCADE
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

-- 5. TABELA CRIPTO_MOEDAS (Depende de USUARIOS)
-- Chave única composta para evitar moedas duplicadas por usuário.
CREATE TABLE cripto_moedas (
  id INT NOT NULL AUTO_INCREMENT,
  email VARCHAR(255) COLLATE utf8mb4_unicode_ci NOT NULL,
  nome_cripto VARCHAR(100) COLLATE utf8mb4_unicode_ci NOT NULL,
  simbolo VARCHAR(10) COLLATE utf8mb4_unicode_ci NOT NULL,
  quantidade DECIMAL(18,8) NOT NULL DEFAULT '0.00000000',
  valor_atual DECIMAL(18,2) NOT NULL DEFAULT '0.00',
  valor_total DECIMAL(18,2) NOT NULL DEFAULT '0.00',
  data_atualizacao TIMESTAMP NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  
  PRIMARY KEY (id),
  UNIQUE KEY uk_cripto_usuario_moeda (email, nome_cripto),
  
  -- Referencia a tabela mestra (usuarios)
  CONSTRAINT fk_cripto_usuario FOREIGN KEY (email) REFERENCES usuarios (email) ON DELETE CASCADE ON UPDATE CASCADE
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;

-- 6. TABELA MOEDAS_ESTRANGEIRAS (Depende de USUARIOS)
-- Chave única composta para evitar moedas duplicadas por usuário.
CREATE TABLE moedas_estrangeiras (
  id INT NOT NULL AUTO_INCREMENT,
  email VARCHAR(255) COLLATE utf8mb4_unicode_ci NOT NULL,
  nome_moeda VARCHAR(50) COLLATE utf8mb4_unicode_ci NOT NULL,
  simbolo VARCHAR(10) COLLATE utf8mb4_unicode_ci NOT NULL,
  quantidade DECIMAL(18,4) NOT NULL DEFAULT '0.0000',
  valor_atual DECIMAL(18,2) NOT NULL DEFAULT '0.00',
  valor_total DECIMAL(18,2) NOT NULL DEFAULT '0.00',
  data_atualizacao TIMESTAMP NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  
  PRIMARY KEY (id),
  UNIQUE KEY uk_moeda_usuario_nome (email, nome_moeda),
  
  -- Referencia a tabela mestra (usuarios)
  CONSTRAINT fk_moeda_usuario FOREIGN KEY (email) REFERENCES usuarios (email) ON DELETE CASCADE ON UPDATE CASCADE
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;

Adicionar usuário e senha no coindo na classe conexao


2°passo= instalar o java fx no Eclipse IDE

3°passo= instalar o Windows builder na maquina
https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&cad=rja&uact=8&ved=2ahUKEwjhxc3lyu2QAxXyLLkGHdVuKmgQFnoECBoQAQ&url=https%3A%2F%2Feclipse.dev%2Fwindowbuilder%2Fdownloads%2F&usg=AOvVaw3H_yTJ9u7g79SB1bmUpRTV&opi=89978449

Exportar o jar contido na pasta lib do javafx-sdk (Não é o mesmo que se instala no eclipse)
https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&cad=rja&uact=8&ved=2ahUKEwiatOD5yu2QAxVDBrkGHcHhGgIQFnoECBcQAQ&url=https%3A%2F%2Fopenjfx.io%2F&usg=AOvVaw2P5dkXLo34-ktNP6qMX847&opi=89978449

Exportar também o jar contido no mysql-connector

4°= cria uma chave api no Gemini
cria uma chave api no coin Market 
Ambas tem bem definido onde coloca-las, a do gemini na TelaResumo 1 e 2 e da Coinmarket em CoinApi

