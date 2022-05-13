  QUESTÃO 01:
SELECT avg(func.salario) AS media_salario, depart.nome_departamento
FROM funcionario fun 
INNER JOIN departamento depart 
ON dpt.numero_departamento = fun.numero_departamento
GROUP BY dpt.nome_departamento;

  QUESTÃO 02:
SELECT avg(func.salario) AS media_salario, fun.sexo
FROM funcionario fun
GROUP BY fun.sexo;

  QUESTÃO 03:
SELECT depart.nome_departamento,
       concat(fun.primeiro_nome, ' ', fun.nome_meio, ' ', fun.ultimo_nome) AS nome_funcionario,
       fun.data_nascimento,
       extract(year FROM age(current_date, fun.data_nascimento)) AS idade,
       fun.salario
FROM departamento depart
INNER JOIN funcionario fun
ON fun.numero_departamento = depart.numero_departamento;

  QUESTÃO 04:
SELECT concat(fun.primeiro_nome, ' ', fun.nome_meio, ' ', fun.ultimo_nome) AS nome_funcionario,
       fun.data_nascimento,
       extract(year FROM age(current_date, fun.data_nascimento)) AS idade,
       fun.salario AS salario_atual,
       (case when (fun.salario < 35) then 20
        else 15
        end) AS taxa_reajuste,
       (case when (fun.salario < 35) then fun.salario + (fun.salario * 0.2)
        else fun.salario + (fun.salario * 0.15)
        end) AS salario_reajustado 
FROM funcionario fun; 

  QUESTÃO 05:
WITH gerente AS (
     SELECT concat(fun.primeiro_nome, ' ', fun.nome_meio, ' ', fun.ultimo_nome) AS nome,
            fun.cpf
     FROM funcionario fun)
SELECT depart.nome_departamento,
       concat(fun.primeiro_nome, ' ', fun.nome_meio, ' ', fun.ultimo_nome) AS nome_funcionario,
       fun.data_nascimento,
       extract(year FROM age(current_date, func.data_nascimento)) AS idade,
       fun.salario,
       ger.nome AS nome_gerente
FROM departamento depart
INNER JOIN funcionario fun 
ON fun.numero_departamento = depart.numero_departamento
INNER JOIN gerente ger ON ger.cpf = depart.cpf_gerente
ORDER BY depart.nome_departamento ASC, fun.salario DESC;

  QUESTÃO 06:
SELECT depart.nome_departamento,
       concat(fun.primeiro_nome, ' ', fun.nome_meio, ' ', fun.ultimo_nome) AS nome_funcionario,
       dep.nome_dependente,
       extract(year FROM age(current_date, dep.data_nascimento)) AS idade_dependente,
       (case
        when (dep.sexo = 'M') then 'Masculino'
        else 'Feminino'
        end) AS sexo_dependente
FROM departamento depart
INNER JOIN funcionario fun
ON fun.numero_departamento = depart.numero_departamento
INNER JOIN dependente dep ON dep.cpf_funcionario = fun.cpf;

  QUESTÃO 07:
WITH dependente_count AS (
    SELECT count(dep) AS count,
           dep.cpf_funcionario
    FROM dependente dep
    GROUP BY dep.cpf_funcionario
)
SELECT depart.nome_departamento,
       concat(func.primeiro_nome, ' ', fun.nome_meio, ' ', fun.ultimo_nome) as nome_funcionario,
       fun.salario
FROM funcionario fun
INNER JOIN departamento depart ON depart.numero_departamento = fun.numero_departamento
LEFT JOIN dependente_count depc ON depc.cpf_funcionario = fun.cpf
WHERE depc.count isnull;

  QUESTÃO 08:
SELECT depart.nome_departamento,
       proj.nome_projeto,
       concat(fun.primeiro_nome, ' ', fun.nome_meio, ' ', fun.ultimo_nome) AS nome_funcionario,
       hrtrab.horas
FROM departamento depart
INNER JOIN projeto proj
ON proj.numero_departamento = depart.numero_departamento
INNER JOIN trabalha_em hrtrab 
ON hrtrab.numero_projeto = proj.numero_projeto
INNER JOIN funcionario f
ON fun.cpf = hrtrab.cpf_funcionario;

  QUESTÃO 09:
SELECT depart.nome_departamento,
       proj.nome_projeto,
       sum(hrtrab.horas)
FROM departamento depart
INNER JOIN projeto proj
ON proj.numero_departamento = depart.numero_departamento
INNER JOIN trabalha_em hrtrab 
ON hrtrab.numero_projeto = proj.numero_projeto
INNER JOIN funcionario fun
ON fun.cpf = hrtrab.cpf_funcionario
GROUP BY depart.nome_departamento, proj.nome_projeto;


  QUESTÃO 10:
SELECT avg(fun.salario) AS media_salario, depart.nome_departamento
FROM funcionario fun
INNER JOIN departamento depart
on depart.numero_departamento = fun.numero_departamento
GROUP BY depart.nome_departamento;


  QUESTÃO 11:
SELECT proj.nome_projeto,
       concat(fun.primeiro_nome, ' ', fun.nome_meio, ' ', fun.ultimo_nome) AS nome_funcionario,
       hrtrab.horas * 50 AS valor
FROM funcionario fun
INNER JOIN trabalha_em hrtrab
ON hrtrab.cpf_funcionario = fun.cpf
INNER JOIN projeto proj
ON proj.numero_projeto  = hrtrab.numero_projeto;

  QUESTÃO 12:
SELECT depart.nome_departamento,
       proj.nome_projeto,
       concat(fun.primeiro_nome, ' ', fun.nome_meio, ' ', fun.ultimo_nome) AS nome_funcionario
FROM departamento depart
INNER JOIN projeto proj
ON proj.numero_departamento = depart.numero_departamento
INNER JOIN trabalha_em hrtrab ON
hrtrab.numero_projeto = proj.numero_projeto
INNER JOIN funcionario fun
ON fun.cpf = hrtrab.cpf_funcionario
WHERE hrtrab.horas isnull OR hrtrab.horas = 0;

  QUESTÃO 13:
SELECT concat(fun.primeiro_nome, ' ', fun.nome_meio, ' ', fun.ultimo_nome) AS nome,
       extract(year FROM age(current_date, fun.data_nascimento)) AS idade,
       fun.sexo
FROM funcionario fun
  UNION
SELECT dep.nome_dependente,
	 extract(year FROM age(current_date, dnasc.data_nascimento)) AS idade,
	 dnasc.sexo
FROM dependente dep
ORDER BY idade;

  QUESTÃO 14:
SELECT depart.nome_departamento, count(fun) AS numero_funcionarios
FROM departamento depart
INNER JOIN funcionario fun
ON depart.numero_departamento = fun.numero_departamento
GROUP BY fun.numero_departamento, depart.nome_departamento;

  QUESTÃO 15:
SELECT depart.nome_departamento,
       proj.nome_projeto,
       concat(fun.primeiro_nome, ' ', fun.nome_meio, ' ', fun.ultimo_nome) AS nome_funcionario
FROM departamento depart
INNER JOIN projeto proj
ON proj.numero_departamento = depart.numero_departamento
LEFT JOIN trabalha_em hrtrab 
ON numero_projeto = proj.numero_projeto
INNER JOIN funcionario fun
ON fun.numero_departamento = depart.numero_departamento;
