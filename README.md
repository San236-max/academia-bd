# academia-bd

Atividade – Mini Mundo (Sistema de Academia)

Este trabalho tem como objetivo apresentar o mini mundo e a modelagem textual do sistema de uma academia, de acordo com os requisitos da atividade solicitada.

Mini Mundo – Sistema de Academia

Uma academia oferece diversas modalidades de treino, como musculação, dança, funcional e alongamento. Cada aluno pode se matricular em uma ou várias modalidades, mediante pagamento mensal. A academia possui professores, cada um responsável por uma ou mais turmas.
Além disso, cada aluno recebe um plano de treino personalizado, montado pelo professor, contendo exercícios, séries e repetições.

O sistema deve armazenar:

Dados dos alunos (nome, endereço, contato, data de nascimento, CPF).

Modalidades oferecidas pela academia.

Professores e suas especialidades.

Turmas (horário, modalidade, professor responsável).

Matrículas dos alunos nessas turmas.

Pagamentos mensais.

Planos de treino individuais.


Entidades Identificadas

1. Aluno


2. Professor


3. Modalidade


4. Turma


5. Matrícula


6. Pagamento


7. PlanoTreino


8. Exercicio


9. ItemTreino



Descrição das Entidades

Aluno: Armazena informações pessoais dos alunos.
Professor: Registra os professores e suas especialidades.
Modalidade: Define as atividades oferecidas pela academia.
Turma: Representa uma modalidade em horário específico.
Matrícula: Liga o aluno a uma turma.
Pagamento: Registra pagamentos dos alunos.
PlanoTreino: Plano individual criado pelo professor.
Exercicio: Lista de exercícios disponíveis.
ItemTreino: Liga exercícios ao plano do aluno.


Repositório criado para entrega da atividade da faculdade de Modelagem de Banco de Dados.
CREATE DATABASE academia;
USE academia;

-- Tabela de planos
CREATE TABLE planos (
    id_plano INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(50),
    valor DECIMAL(10,2)
);

-- Tabela de alunos
CREATE TABLE alunos (
    id_aluno INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(100),
    idade INT,
    id_plano INT,
    FOREIGN KEY (id_plano) REFERENCES planos(id_plano)
);

-- Tabela de instrutores
CREATE TABLE instrutores (
    id_instrutor INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(100),
    especialidade VARCHAR(50)
);

-- Tabela de atividades
CREATE TABLE atividades (
    id_atividade INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(50),
    carga_horaria INT
);

-- Tabela de treinos
CREATE TABLE treinos (
    id_treino INT AUTO_INCREMENT PRIMARY KEY,
    id_aluno INT,
    id_instrutor INT,
    id_atividade INT,
    data DATE,
    FOREIGN KEY (id_aluno) REFERENCES alunos(id_aluno),
    FOREIGN KEY (id_instrutor) REFERENCES instrutores(id_instrutor),
    FOREIGN KEY (id_atividade) REFERENCES atividades(id_atividade)
);

-- Planos
INSERT INTO planos (nome, valor) VALUES
('Mensal', 120.00),
('Trimestral', 300.00),
('Anual', 900.00);

-- Alunos
INSERT INTO alunos (nome, idade, id_plano) VALUES
('Carlos Silva', 25, 1),
('Ana Costa', 30, 2),
('Bruno Mendes', 22, 1),
('Julia Teixeira', 27, 3);

-- Instrutores
INSERT INTO instrutores (nome, especialidade) VALUES
('Marcos Souza', 'Musculação'),
('Fernanda Lima', 'Aeróbico'),
('João Ribeiro', 'Funcional');

-- Atividades
INSERT INTO atividades (nome, carga_horaria) VALUES
('Musculação', 60),
('Esteira', 45),
('Funcional', 50),
('Spinning', 40);

-- Treinos
INSERT INTO treinos (id_aluno, id_instrutor, id_atividade, data) VALUES
(1, 1, 1, '2024-10-10'),
(2, 2, 2, '2024-10-11'),
(3, 3, 3, '2024-10-12'),
(4, 1, 1, '2024-10-13'),
(1, 2, 4, '2024-10-14');

- SELECT 1: Todos os alunos
SELECT * FROM alunos;

-- SELECT 2: Alunos filtrados por idade
SELECT nome, idade FROM alunos
WHERE idade > 25;

-- SELECT 3: Planos ordenados por valor
SELECT * FROM planos
ORDER BY valor DESC;

-- SELECT 4: Limitar resultados (3 primeiros treinos)
SELECT * FROM treinos
LIMIT 3;

-- SELECT 5: JOIN - aluno, instrutor e atividade
SELECT a.nome AS aluno,
       i.nome AS instrutor,
       t.data,
       atv.nome AS atividade
FROM treinos t
JOIN alunos a ON a.id_aluno = t.id_aluno
JOIN instrutores i ON i.id_instrutor = t.id_instrutor
JOIN atividades atv ON atv.id_atividade = t.id_atividade;

-- UPDATE 1: Alterar o plano de um aluno
UPDATE alunos
SET id_plano = 3
WHERE id_aluno = 1;

-- UPDATE 2: Atualizar especialidade de instrutor
UPDATE instrutores
SET especialidade = 'Crossfit'
WHERE id_instrutor = 3;

-- UPDATE 3: Modificar carga horária de uma atividade
UPDATE atividades
SET carga_horaria = 55
WHERE id_atividade = 3;

-- DELETE 1: Apagar treino antigo
DELETE FROM treinos
WHERE data < '2024-10-11';

-- DELETE 2: Remover aluno específico
DELETE FROM alunos
WHERE nome = 'Bruno Mendes';

-- DELETE 3: Excluir atividade descontinuada
DELETE FROM atividades
WHERE nome = 'Spinning';
