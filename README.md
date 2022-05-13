--QUESTÃO 01:
SELECT avg(func.salario) AS media_salario, dpt.nome_departamento
FROM funcionario func 
INNER JOIN departamento dpt 
ON dpt.numero_departamento = func.numero_departamento
GROUP BY dpt.nome_departamento;

-- QUESTÃO 02:
SELECT avg(func.salario) AS media_salario, func.sexo
FROM funcionario func
GROUP BY func.sexo;

-- QUESTÃO 03:
SELECT dpt.nome_departamento,
       concat(func.primeiro_nome, ' ', func.nome_meio, ' ', func.ultimo_nome) AS nome_funcionario,
       func.data_nascimento,
       extract(year FROM age(current_date, func.data_nascimento)) AS idade,
       func.salario
FROM departamento dpt
INNER JOIN funcionario func
ON func.numero_departamento = dpt.numero_departamento;

-- QUESTÃO 04:
SELECT concat(func.primeiro_nome, ' ', func.nome_meio, ' ', func.ultimo_nome) AS nome_funcionario,
       func.data_nascimento,
       extract(year FROM age(current_date, func.data_nascimento)) AS idade,
       func.salario AS salario_atual,
       (case when (func.salario < 35) then 20
        else 15
        end) AS taxa_reajuste,
       (case when (func.salario < 35) then func.salario + (func.salario * 0.2)
        else func.salario + (func.salario * 0.15)
        end) AS salario_reajustado 
FROM funcionario func; 

-- QUESTÃO 05:
WITH gerente AS (
     SELECT concat(func.primeiro_nome, ' ', func.nome_meio, ' ', func.ultimo_nome) AS nome,
            func.cpf
     FROM funcionario func)
SELECT dpt.nome_departamento,
       concat(func.primeiro_nome, ' ', func.nome_meio, ' ', func.ultimo_nome) AS nome_funcionario,
       func.data_nascimento,
       extract(year FROM age(current_date, func.data_nascimento)) AS idade,
       func.salario,
       ger.nome AS nome_gerente
FROM departamento dpt
INNER JOIN funcionario func 
ON func.numero_departamento = dpt.numero_departamento
INNER JOIN gerente ger ON ger.cpf = dpt.cpf_gerente
ORDER BY dpt.nome_departamento ASC, func.salario DESC;

-- QUESTÃO 06:
SELECT dpt.nome_departamento,
       concat(func.primeiro_nome, ' ', func.nome_meio, ' ', func.ultimo_nome) AS nome_funcionario,
       dep.nome_dependente,
       extract(year FROM age(current_date, dep.data_nascimento)) AS idade_dependente,
       (case
        when (dep.sexo = 'M') then 'Masculino'
        else 'Feminino'
        end) AS sexo_dependente
FROM departamento dpt
INNER JOIN funcionario func
ON func.numero_departamento = dpt.numero_departamento
INNER JOIN dependente dep ON dep.cpf_funcionario = func.cpf;

-- QUESTÃO 07:
WITH dependente_count AS (
    SELECT count(dep) AS count,
           dep.cpf_funcionario
    FROM dependente dep
    GROUP BY dep.cpf_funcionario
)
SELECT dpt.nome_departamento,
       concat(func.primeiro_nome, ' ', func.nome_meio, ' ', func.ultimo_nome) as nome_funcionario,
       func.salario
FROM funcionario func
INNER JOIN departamento dpt ON dpt.numero_departamento = func.numero_departamento
LEFT JOIN dependente_count depc ON depc.cpf_funcionario = func.cpf
WHERE depc.count isnull;

-- QUESTÃO 08:
SELECT dpt.nome_departamento,
       proj.nome_projeto,
       concat(func.primeiro_nome, ' ', func.nome_meio, ' ', func.ultimo_nome) AS nome_funcionario,
       hrtrab.horas
FROM departamento dpt
INNER JOIN projeto proj
ON proj.numero_departamento = dpt.numero_departamento
INNER JOIN trabalha_em hrtrab 
ON hrtrab.numero_projeto = proj.numero_projeto
INNER JOIN funcionario f
ON func.cpf = hrtrab.cpf_funcionario;

-- QUESTÃO 09:
SELECT dpt.nome_departamento,
       proj.nome_projeto,
       sum(hrtrab.horas)
FROM departamento dpt
INNER JOIN projeto proj
ON proj.numero_departamento = dpt.numero_departamento
INNER JOIN trabalha_em hrtrab 
ON hrtrab.numero_projeto = proj.numero_projeto
INNER JOIN funcionario func
ON func.cpf = hrtrab.cpf_funcionario
GROUP BY dpt.nome_departamento, proj.nome_projeto;


-- QUESTÃO 10:
SELECT avg(func.salario) AS media_salario, dpt.nome_departamento
FROM funcionario func
INNER JOIN departamento dpt
on dpt.numero_departamento = func.numero_departamento
GROUP BY dpt.nome_departamento;


-- QUESTÃO 11:
SELECT proj.nome_projeto,
       concat(func.primeiro_nome, ' ', func.nome_meio, ' ', func.ultimo_nome) AS nome_funcionario,
       hrtrab.horas * 50 AS valor
FROM funcionario func
INNER JOIN trabalha_em hrtrab
ON hrtrab.cpf_funcionario = func.cpf
INNER JOIN projeto proj
ON proj.numero_projeto  = hrtrab.numero_projeto;

-- QUESTÃO 12:
SELECT dpt.nome_departamento,
       proj.nome_projeto,
       concat(func.primeiro_nome, ' ', func.nome_meio, ' ', func.ultimo_nome) AS nome_funcionario
FROM departamento dpt
INNER JOIN projeto proj
ON proj.numero_departamento = dpt.numero_departamento
INNER JOIN trabalha_em hrtrab ON
hrtrab.numero_projeto = proj.numero_projeto
INNER JOIN funcionario func
ON func.cpf = hrtrab.cpf_funcionario
WHERE hrtrab.horas isnull OR hrtrab.horas = 0;

-- QUESTÃO 13:
SELECT concat(func.primeiro_nome, ' ', func.nome_meio, ' ', func.ultimo_nome) AS nome,
       extract(year FROM age(current_date, func.data_nascimento)) AS idade,
       func.sexo
FROM funcionario func 
UNION
SELECT dep.nome_dependente,
	   extract(year FROM age(current_date, dnasc.data_nascimento)) AS idade,
	   dnasc.sexo
FROM dependente dep
ORDER BY idade;

-- QUESTÃO 14:
SELECT dpt.nome_departamento, count(func) AS numero_funcionarios
FROM departamento dpt 
INNER JOIN funcionario func
ON dpt.numero_departamento = func.numero_departamento
GROUP BY func.numero_departamento, dpt.nome_departamento;

-- QUESTÃO 15:
SELECT dpt.nome_departamento,
       proj.nome_projeto,
       concat(func.primeiro_nome, ' ', func.nome_meio, ' ', func.ultimo_nome) AS nome_funcionario
FROM departamento dpt
INNER JOIN projeto proj
ON proj.numero_departamento = dpt.numero_departamento
LEFT JOIN trabalha_em hrtrab 
ON numero_projeto = proj.numero_projeto
INNER JOIN funcionario func
ON func.numero_departamento = dpt.numero_departamento;
